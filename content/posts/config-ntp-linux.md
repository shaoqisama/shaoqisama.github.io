+++
date = '2024-12-19T14:37:38+08:00'
draft = false
title = 'Config Ntp Linux'
tags = ['coding', 'linux']
ShowToc = true
+++
## 1. **Understanding Chrony vs NTP**
- **Chrony**: A fast, flexible NTP implementation for Linux, ideal for intermittent or variable clock systems.
- **ntpq**: A tool for querying traditional NTP servers (`ntpd`).

| **Feature**            | **Chrony (chronyc)**                               | **ntpq**                                  |
|------------------------|----------------------------------------------------|-------------------------------------------|
| **Implementation**      | Part of the Chrony NTP implementation             | Part of the ntpd implementation          |
| **Focus**               | Lightweight, fast synchronization                 | Detailed monitoring of ntpd              |
| **Efficiency**          | Better for unstable or intermittent networks      | Less efficient in unstable networks      |
| **Time Sources**        | Supports NTP, GPS, PPS, and more                  | Primarily NTP                            |
| **Commands**            | Uses chronyc for querying and control             | Uses ntpq for querying ntpd              |
| **Suitability**         | Ideal for modern systems and VMs                  | Traditional systems with ntpd            |

<!--more-->
---

## 2. **Checking Chrony Configuration**
- **Verify NTP Server**:
  - `chronyc sources`: Displays NTP sources and their status.
    - sample example:
      - ```
        210 Number of sources = 2
        MS Name/IP address         Stratum Poll Reach LastRx Last sample
        ==============================================================================
        ^* time.google.com               1   6   377    34   +0ns[  +0ns] +/-  17ms
        ^- ntp.example.com               2   6   377    45   -2ms[  -3ms] +/-  22ms
        ```
    - **MS**: Indicates the status of the server:
      - `*`: The selected synchronization source.
      - `+`: A candidate for synchronization.
      - `-`: A valid but currently unused source.
      - `?`: A source that is unreachable.
    - **Name/IP address**: The hostname or IP of the NTP server.
    - **Stratum**: The stratum level of the server (lower is better; 1 is the best).
    - **Reach**: The reachability status (a value of 377 indicates the server is fully reachable).
    - **LastRx**: Time since the last response was received.
    - **Last sample**: The offset and delay of the server.


  - `chronyc tracking`: Provides synchronization details.
    - sample example
        - ```   Reference ID    : time.google.com (216.239.35.0)
                Stratum         : 2
                Ref time (UTC)  : Wed Dec 19 12:34:56 2024
                System time     : 0.000002 seconds slow of NTP time
                Last offset     : -0.000001 seconds
                RMS offset      : 0.000002 seconds
                Frequency       : 5.123 ppm fast
                Residual freq   : 0.001 ppm
                Skew            : 0.002 ppm
                Root delay      : 0.017 ms
                Root dispersion : 0.005 ms
                Update interval : 64.0 seconds
                Leap status     : Normal
                ```

---

## 3. **Ansible Playbooks for Chrony**
### **Short Playbook: Configure Chrony NTP Server**
```yaml
---
- name: Configure Chrony NTP server
  hosts: all
  become: yes
  tasks:
    - name: Install Chrony
      yum:
        name: chrony
        state: present

    - name: Backup configuration
      copy:
        src: /etc/chrony.conf
        dest: /etc/chrony.conf.bak
        remote_src: yes

    - name: Comment out default NTP pool
      lineinfile:
        path: /etc/chrony.conf
        regexp: '^pool 2\.rhel\.pool\.ntp\.org iburst'
        line: '# pool 2.rhel.pool.ntp.org iburst'

    - name: Add custom NTP server
      lineinfile:
        path: /etc/chrony.conf
        regexp: '^server .* iburst'
        line: 'server 192.168.0.1 iburst' # replace your ntp server

    - name: Restart Chrony
      service:
        name: chronyd
        state: restarted

    - name: Save service status locally
      command: systemctl status chronyd
      register: status
      delegate_to: localhost
      args:
        dest: "./chrony_status.txt"

    - name: Save sources locally
      command: chronyc sources
      register: sources
      delegate_to: localhost
      args:
        dest: "./chrony_sources.txt"
