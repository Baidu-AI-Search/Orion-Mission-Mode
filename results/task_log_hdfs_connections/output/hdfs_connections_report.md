# HDFS DataNode Log — Connection & Communication Report
**Log file:** `hdfs_datanode.log`  **Total log entries parsed:** 2000  **Unique IP addresses observed:** 202  **Components present:** DataNode, FSNamesystem, PacketResponder (counted under DataNode)
---
## 1. Network Topology — Unique IP Addresses by Role
- **Source-only nodes** (only ever seen as origin/initiator): **0**
- **Destination-only nodes** (only ever seen as target): **0**
- **Both source & destination** (peer nodes participating in bidirectional transfers): **202**
  - 10.250.10.100, 10.250.10.144, 10.250.10.176, 10.250.10.213, 10.250.10.223, 10.250.10.6, 10.250.11.100, 10.250.11.194, 10.250.11.53, 10.250.11.85, 10.250.13.188, 10.250.13.240, 10.250.14.143, 10.250.14.196, 10.250.14.224, 10.250.14.38, 10.250.15.101, 10.250.15.198, 10.250.15.240, 10.250.15.67, 10.250.17.177, 10.250.17.225, 10.250.18.114, 10.250.19.102, 10.250.19.16, 10.250.19.227, 10.250.5.161, 10.250.5.237, 10.250.6.191, 10.250.6.214, 10.250.6.223, 10.250.6.4, 10.250.7.146, 10.250.7.230, 10.250.7.244, 10.250.7.32, 10.250.7.96, 10.250.9.207, 10.251.105.189, 10.251.106.10, 10.251.106.214, 10.251.106.37, 10.251.106.50, 10.251.107.19, 10.251.107.196, 10.251.107.227, 10.251.107.242, 10.251.107.50, 10.251.107.98, 10.251.109.209, 10.251.109.236, 10.251.110.160, 10.251.110.196, 10.251.110.68, 10.251.110.8, 10.251.111.130, 10.251.111.209, 10.251.111.228, 10.251.111.37, 10.251.111.80, 10.251.121.224, 10.251.122.38, 10.251.122.65, 10.251.122.79, 10.251.123.1, 10.251.123.132, 10.251.123.195, 10.251.123.20, 10.251.123.33, 10.251.123.99, 10.251.125.174, 10.251.125.193, 10.251.125.237, 10.251.126.22, 10.251.126.227, 10.251.126.255, 10.251.126.5, 10.251.126.83, 10.251.127.191, 10.251.127.243, 10.251.127.47, 10.251.193.175, 10.251.193.224, 10.251.194.102, 10.251.194.129, 10.251.194.147, 10.251.194.213, 10.251.194.245, 10.251.195.33, 10.251.195.52, 10.251.195.70, 10.251.197.161, 10.251.197.226, 10.251.198.196, 10.251.198.33, 10.251.199.150, 10.251.199.159, 10.251.199.19, 10.251.199.225, 10.251.199.245, 10.251.199.86, 10.251.201.204, 10.251.202.181, 10.251.202.209, 10.251.203.129, 10.251.203.149, 10.251.203.166, 10.251.203.179, 10.251.203.246, 10.251.203.4, 10.251.203.80, 10.251.214.112, 10.251.214.130, 10.251.214.175, 10.251.214.18, 10.251.214.225, 10.251.214.32, 10.251.214.67, 10.251.215.16, 10.251.215.192, 10.251.215.50, 10.251.215.70, 10.251.25.237, 10.251.26.131, 10.251.26.177, 10.251.26.8, 10.251.27.63, 10.251.29.239, 10.251.30.101, 10.251.30.134, 10.251.30.179, 10.251.30.6, 10.251.30.85, 10.251.31.160, 10.251.31.180, 10.251.31.242, 10.251.31.5, 10.251.31.85, 10.251.35.1, 10.251.37.240, 10.251.38.197, 10.251.38.214, 10.251.38.53, 10.251.39.144, 10.251.39.160, 10.251.39.179, 10.251.39.192, 10.251.39.209, 10.251.39.242, 10.251.39.64, 10.251.42.16, 10.251.42.191, 10.251.42.207, 10.251.42.246, 10.251.42.84, 10.251.42.9, 10.251.43.115, 10.251.43.147, 10.251.43.192, 10.251.43.21, 10.251.43.210, 10.251.65.203, 10.251.65.237, 10.251.66.102, 10.251.66.192, 10.251.66.3, 10.251.66.63, 10.251.67.113, 10.251.67.211, 10.251.67.225, 10.251.67.4, 10.251.70.112, 10.251.70.211, 10.251.70.37, 10.251.70.5, 10.251.71.146, 10.251.71.16, 10.251.71.193, 10.251.71.240, 10.251.71.68, 10.251.71.97, 10.251.73.188, 10.251.73.220, 10.251.74.134, 10.251.74.192, 10.251.74.227, 10.251.74.79, 10.251.75.143, 10.251.75.163, 10.251.75.228, 10.251.75.49, 10.251.75.79, 10.251.89.155, 10.251.90.134, 10.251.90.239, 10.251.90.64, 10.251.90.81, 10.251.91.15, 10.251.91.159, 10.251.91.229, 10.251.91.32, 10.251.91.84

### Full IP Roster (sorted) with Appearance Counts
| IP Address | Subnet | Total Mentions | Role |
|---|---|---|---|
| 10.250.5.161 | 10.250.x.x | 12 | src/dst |
| 10.250.5.237 | 10.250.x.x | 8 | src/dst |
| 10.250.6.4 | 10.250.x.x | 12 | src/dst |
| 10.250.6.191 | 10.250.x.x | 12 | src/dst |
| 10.250.6.214 | 10.250.x.x | 9 | src/dst |
| 10.250.6.223 | 10.250.x.x | 8 | src/dst |
| 10.250.7.32 | 10.250.x.x | 10 | src/dst |
| 10.250.7.96 | 10.250.x.x | 14 | src/dst |
| 10.250.7.146 | 10.250.x.x | 14 | src/dst |
| 10.250.7.230 | 10.250.x.x | 6 | src/dst |
| 10.250.7.244 | 10.250.x.x | 14 | src/dst |
| 10.250.9.207 | 10.250.x.x | 16 | src/dst |
| 10.250.10.6 | 10.250.x.x | 46 | src/dst |
| 10.250.10.100 | 10.250.x.x | 12 | src/dst |
| 10.250.10.144 | 10.250.x.x | 12 | src/dst |
| 10.250.10.176 | 10.250.x.x | 14 | src/dst |
| 10.250.10.213 | 10.250.x.x | 12 | src/dst |
| 10.250.10.223 | 10.250.x.x | 12 | src/dst |
| 10.250.11.53 | 10.250.x.x | 14 | src/dst |
| 10.250.11.85 | 10.250.x.x | 12 | src/dst |
| 10.250.11.100 | 10.250.x.x | 93 | src/dst |
| 10.250.11.194 | 10.250.x.x | 12 | src/dst |
| 10.250.13.188 | 10.250.x.x | 18 | src/dst |
| 10.250.13.240 | 10.250.x.x | 12 | src/dst |
| 10.250.14.38 | 10.250.x.x | 8 | src/dst |
| 10.250.14.143 | 10.250.x.x | 14 | src/dst |
| 10.250.14.196 | 10.250.x.x | 16 | src/dst |
| 10.250.14.224 | 10.250.x.x | 54 | src/dst |
| 10.250.15.67 | 10.250.x.x | 18 | src/dst |
| 10.250.15.101 | 10.250.x.x | 12 | src/dst |
| 10.250.15.198 | 10.250.x.x | 12 | src/dst |
| 10.250.15.240 | 10.250.x.x | 16 | src/dst |
| 10.250.17.177 | 10.250.x.x | 14 | src/dst |
| 10.250.17.225 | 10.250.x.x | 12 | src/dst |
| 10.250.18.114 | 10.250.x.x | 12 | src/dst |
| 10.250.19.16 | 10.250.x.x | 14 | src/dst |
| 10.250.19.102 | 10.250.x.x | 15 | src/dst |
| 10.250.19.227 | 10.250.x.x | 10 | src/dst |
| 10.251.25.237 | 10.251.x.x | 10 | src/dst |
| 10.251.26.8 | 10.251.x.x | 12 | src/dst |
| 10.251.26.131 | 10.251.x.x | 10 | src/dst |
| 10.251.26.177 | 10.251.x.x | 14 | src/dst |
| 10.251.27.63 | 10.251.x.x | 12 | src/dst |
| 10.251.29.239 | 10.251.x.x | 14 | src/dst |
| 10.251.30.6 | 10.251.x.x | 10 | src/dst |
| 10.251.30.85 | 10.251.x.x | 10 | src/dst |
| 10.251.30.101 | 10.251.x.x | 10 | src/dst |
| 10.251.30.134 | 10.251.x.x | 12 | src/dst |
| 10.251.30.179 | 10.251.x.x | 14 | src/dst |
| 10.251.31.5 | 10.251.x.x | 43 | src/dst |
| 10.251.31.85 | 10.251.x.x | 18 | src/dst |
| 10.251.31.160 | 10.251.x.x | 14 | src/dst |
| 10.251.31.180 | 10.251.x.x | 12 | src/dst |
| 10.251.31.242 | 10.251.x.x | 14 | src/dst |
| 10.251.35.1 | 10.251.x.x | 12 | src/dst |
| 10.251.37.240 | 10.251.x.x | 14 | src/dst |
| 10.251.38.53 | 10.251.x.x | 16 | src/dst |
| 10.251.38.197 | 10.251.x.x | 16 | src/dst |
| 10.251.38.214 | 10.251.x.x | 12 | src/dst |
| 10.251.39.64 | 10.251.x.x | 12 | src/dst |
| 10.251.39.144 | 10.251.x.x | 16 | src/dst |
| 10.251.39.160 | 10.251.x.x | 16 | src/dst |
| 10.251.39.179 | 10.251.x.x | 74 | src/dst |
| 10.251.39.192 | 10.251.x.x | 10 | src/dst |
| 10.251.39.209 | 10.251.x.x | 14 | src/dst |
| 10.251.39.242 | 10.251.x.x | 14 | src/dst |
| 10.251.42.9 | 10.251.x.x | 20 | src/dst |
| 10.251.42.16 | 10.251.x.x | 14 | src/dst |
| 10.251.42.84 | 10.251.x.x | 14 | src/dst |
| 10.251.42.191 | 10.251.x.x | 12 | src/dst |
| 10.251.42.207 | 10.251.x.x | 10 | src/dst |
| 10.251.42.246 | 10.251.x.x | 12 | src/dst |
| 10.251.43.21 | 10.251.x.x | 10 | src/dst |
| 10.251.43.115 | 10.251.x.x | 14 | src/dst |
| 10.251.43.147 | 10.251.x.x | 14 | src/dst |
| 10.251.43.192 | 10.251.x.x | 12 | src/dst |
| 10.251.43.210 | 10.251.x.x | 14 | src/dst |
| 10.251.65.203 | 10.251.x.x | 12 | src/dst |
| 10.251.65.237 | 10.251.x.x | 12 | src/dst |
| 10.251.66.3 | 10.251.x.x | 12 | src/dst |
| 10.251.66.63 | 10.251.x.x | 14 | src/dst |
| 10.251.66.102 | 10.251.x.x | 14 | src/dst |
| 10.251.66.192 | 10.251.x.x | 12 | src/dst |
| 10.251.67.4 | 10.251.x.x | 16 | src/dst |
| 10.251.67.113 | 10.251.x.x | 14 | src/dst |
| 10.251.67.211 | 10.251.x.x | 16 | src/dst |
| 10.251.67.225 | 10.251.x.x | 14 | src/dst |
| 10.251.70.5 | 10.251.x.x | 10 | src/dst |
| 10.251.70.37 | 10.251.x.x | 12 | src/dst |
| 10.251.70.112 | 10.251.x.x | 12 | src/dst |
| 10.251.70.211 | 10.251.x.x | 24 | src/dst |
| 10.251.71.16 | 10.251.x.x | 14 | src/dst |
| 10.251.71.68 | 10.251.x.x | 14 | src/dst |
| 10.251.71.97 | 10.251.x.x | 12 | src/dst |
| 10.251.71.146 | 10.251.x.x | 16 | src/dst |
| 10.251.71.193 | 10.251.x.x | 37 | src/dst |
| 10.251.71.240 | 10.251.x.x | 34 | src/dst |
| 10.251.73.188 | 10.251.x.x | 14 | src/dst |
| 10.251.73.220 | 10.251.x.x | 12 | src/dst |
| 10.251.74.79 | 10.251.x.x | 48 | src/dst |
| 10.251.74.134 | 10.251.x.x | 12 | src/dst |
| 10.251.74.192 | 10.251.x.x | 16 | src/dst |
| 10.251.74.227 | 10.251.x.x | 14 | src/dst |
| 10.251.75.49 | 10.251.x.x | 10 | src/dst |
| 10.251.75.79 | 10.251.x.x | 12 | src/dst |
| 10.251.75.143 | 10.251.x.x | 12 | src/dst |
| 10.251.75.163 | 10.251.x.x | 10 | src/dst |
| 10.251.75.228 | 10.251.x.x | 12 | src/dst |
| 10.251.89.155 | 10.251.x.x | 13 | src/dst |
| 10.251.90.64 | 10.251.x.x | 29 | src/dst |
| 10.251.90.81 | 10.251.x.x | 18 | src/dst |
| 10.251.90.134 | 10.251.x.x | 12 | src/dst |
| 10.251.90.239 | 10.251.x.x | 16 | src/dst |
| 10.251.91.15 | 10.251.x.x | 12 | src/dst |
| 10.251.91.32 | 10.251.x.x | 12 | src/dst |
| 10.251.91.84 | 10.251.x.x | 12 | src/dst |
| 10.251.91.159 | 10.251.x.x | 16 | src/dst |
| 10.251.91.229 | 10.251.x.x | 18 | src/dst |
| 10.251.105.189 | 10.251.x.x | 10 | src/dst |
| 10.251.106.10 | 10.251.x.x | 13 | src/dst |
| 10.251.106.37 | 10.251.x.x | 14 | src/dst |
| 10.251.106.50 | 10.251.x.x | 16 | src/dst |
| 10.251.106.214 | 10.251.x.x | 16 | src/dst |
| 10.251.107.19 | 10.251.x.x | 43 | src/dst |
| 10.251.107.50 | 10.251.x.x | 12 | src/dst |
| 10.251.107.98 | 10.251.x.x | 12 | src/dst |
| 10.251.107.196 | 10.251.x.x | 20 | src/dst |
| 10.251.107.227 | 10.251.x.x | 12 | src/dst |
| 10.251.107.242 | 10.251.x.x | 12 | src/dst |
| 10.251.109.209 | 10.251.x.x | 20 | src/dst |
| 10.251.109.236 | 10.251.x.x | 14 | src/dst |
| 10.251.110.8 | 10.251.x.x | 20 | src/dst |
| 10.251.110.68 | 10.251.x.x | 12 | src/dst |
| 10.251.110.160 | 10.251.x.x | 18 | src/dst |
| 10.251.110.196 | 10.251.x.x | 14 | src/dst |
| 10.251.111.37 | 10.251.x.x | 14 | src/dst |
| 10.251.111.80 | 10.251.x.x | 14 | src/dst |
| 10.251.111.130 | 10.251.x.x | 12 | src/dst |
| 10.251.111.209 | 10.251.x.x | 31 | src/dst |
| 10.251.111.228 | 10.251.x.x | 14 | src/dst |
| 10.251.121.224 | 10.251.x.x | 10 | src/dst |
| 10.251.122.38 | 10.251.x.x | 14 | src/dst |
| 10.251.122.65 | 10.251.x.x | 10 | src/dst |
| 10.251.122.79 | 10.251.x.x | 14 | src/dst |
| 10.251.123.1 | 10.251.x.x | 16 | src/dst |
| 10.251.123.20 | 10.251.x.x | 12 | src/dst |
| 10.251.123.33 | 10.251.x.x | 14 | src/dst |
| 10.251.123.99 | 10.251.x.x | 10 | src/dst |
| 10.251.123.132 | 10.251.x.x | 18 | src/dst |
| 10.251.123.195 | 10.251.x.x | 14 | src/dst |
| 10.251.125.174 | 10.251.x.x | 14 | src/dst |
| 10.251.125.193 | 10.251.x.x | 16 | src/dst |
| 10.251.125.237 | 10.251.x.x | 14 | src/dst |
| 10.251.126.5 | 10.251.x.x | 14 | src/dst |
| 10.251.126.22 | 10.251.x.x | 10 | src/dst |
| 10.251.126.83 | 10.251.x.x | 16 | src/dst |
| 10.251.126.227 | 10.251.x.x | 10 | src/dst |
| 10.251.126.255 | 10.251.x.x | 18 | src/dst |
| 10.251.127.47 | 10.251.x.x | 14 | src/dst |
| 10.251.127.191 | 10.251.x.x | 14 | src/dst |
| 10.251.127.243 | 10.251.x.x | 14 | src/dst |
| 10.251.193.175 | 10.251.x.x | 10 | src/dst |
| 10.251.193.224 | 10.251.x.x | 16 | src/dst |
| 10.251.194.102 | 10.251.x.x | 16 | src/dst |
| 10.251.194.129 | 10.251.x.x | 12 | src/dst |
| 10.251.194.147 | 10.251.x.x | 18 | src/dst |
| 10.251.194.213 | 10.251.x.x | 12 | src/dst |
| 10.251.194.245 | 10.251.x.x | 14 | src/dst |
| 10.251.195.33 | 10.251.x.x | 12 | src/dst |
| 10.251.195.52 | 10.251.x.x | 10 | src/dst |
| 10.251.195.70 | 10.251.x.x | 20 | src/dst |
| 10.251.197.161 | 10.251.x.x | 16 | src/dst |
| 10.251.197.226 | 10.251.x.x | 88 | src/dst |
| 10.251.198.33 | 10.251.x.x | 6 | src/dst |
| 10.251.198.196 | 10.251.x.x | 16 | src/dst |
| 10.251.199.19 | 10.251.x.x | 12 | src/dst |
| 10.251.199.86 | 10.251.x.x | 8 | src/dst |
| 10.251.199.150 | 10.251.x.x | 12 | src/dst |
| 10.251.199.159 | 10.251.x.x | 12 | src/dst |
| 10.251.199.225 | 10.251.x.x | 14 | src/dst |
| 10.251.199.245 | 10.251.x.x | 14 | src/dst |
| 10.251.201.204 | 10.251.x.x | 12 | src/dst |
| 10.251.202.181 | 10.251.x.x | 12 | src/dst |
| 10.251.202.209 | 10.251.x.x | 16 | src/dst |
| 10.251.203.4 | 10.251.x.x | 12 | src/dst |
| 10.251.203.80 | 10.251.x.x | 8 | src/dst |
| 10.251.203.129 | 10.251.x.x | 16 | src/dst |
| 10.251.203.149 | 10.251.x.x | 14 | src/dst |
| 10.251.203.166 | 10.251.x.x | 10 | src/dst |
| 10.251.203.179 | 10.251.x.x | 12 | src/dst |
| 10.251.203.246 | 10.251.x.x | 10 | src/dst |
| 10.251.214.18 | 10.251.x.x | 12 | src/dst |
| 10.251.214.32 | 10.251.x.x | 14 | src/dst |
| 10.251.214.67 | 10.251.x.x | 18 | src/dst |
| 10.251.214.112 | 10.251.x.x | 12 | src/dst |
| 10.251.214.130 | 10.251.x.x | 12 | src/dst |
| 10.251.214.175 | 10.251.x.x | 12 | src/dst |
| 10.251.214.225 | 10.251.x.x | 12 | src/dst |
| 10.251.215.16 | 10.251.x.x | 53 | src/dst |
| 10.251.215.50 | 10.251.x.x | 22 | src/dst |
| 10.251.215.70 | 10.251.x.x | 12 | src/dst |
| 10.251.215.192 | 10.251.x.x | 10 | src/dst |

---
## 2. Subnet Analysis
IPs are grouped by `/16` (first two octets), matching the `10.250.x.x` vs `10.251.x.x` convention in the log.
| Subnet | # Nodes | % of Unique IPs |
|---|---|---|
| `10.250.x.x` | 38 | 18.8% |
| `10.251.x.x` | 164 | 81.2% |

### Observations
- The cluster spans **two primary /16 subnets** (`10.250.0.0/16` and `10.251.0.0/16`), which is typical of a multi-rack deployment where each /16 corresponds to a rack or an availability zone.
- Cross-subnet (inter-rack) replication traffic is visible: `Transmitted block ...` lines frequently show `10.250.x → 10.251.x` transfers, consistent with Hadoop's **rack-aware replica placement** (one replica on same rack, two on a different rack).

---
## 3. Most Active Nodes — Top 10 IPs by Frequency
Frequency counts every log line in which the IP appears (in any role).
| Rank | IP Address | Subnet | Appearances | Likely Role |
|---|---|---|---|---|
| 1 | 10.250.11.100 | 10.250.x.x | 93 | DataNode (this log's host — appears in nearly every local transfer event) |
| 2 | 10.251.197.226 | 10.251.x.x | 88 | DataNode |
| 3 | 10.251.39.179 | 10.251.x.x | 74 | DataNode |
| 4 | 10.250.14.224 | 10.250.x.x | 54 | DataNode |
| 5 | 10.251.215.16 | 10.251.x.x | 53 | DataNode |
| 6 | 10.251.74.79 | 10.251.x.x | 48 | DataNode |
| 7 | 10.250.10.6 | 10.250.x.x | 46 | DataNode |
| 8 | 10.251.107.19 | 10.251.x.x | 43 | DataNode |
| 9 | 10.251.31.5 | 10.251.x.x | 43 | DataNode |
| 10 | 10.251.71.193 | 10.251.x.x | 37 | DataNode |

---
## 4. Communication Patterns — Top Node Pairs
### Top Undirected Pairs (most frequent bilateral communication)
| Rank | Node A | Node B | # Interactions | Subnet Relation |
|---|---|---|---|---|
| 1 | 10.250.14.224 | 10.251.215.16 | 3 | cross-subnet (inter-rack) |
| 2 | 10.251.215.16 | 10.251.74.79 | 3 | same subnet |
| 3 | 10.251.107.19 | 10.251.31.5 | 3 | same subnet |
| 4 | 10.251.31.5 | 10.251.90.64 | 3 | same subnet |
| 5 | 10.250.14.224 | 10.251.71.193 | 2 | cross-subnet (inter-rack) |
| 6 | 10.251.107.19 | 10.251.215.16 | 2 | same subnet |
| 7 | 10.251.107.19 | 10.251.71.240 | 2 | same subnet |
| 8 | 10.250.11.100 | 10.250.19.102 | 1 | same subnet |
| 9 | 10.250.19.102 | 10.251.111.209 | 1 | cross-subnet (inter-rack) |
| 10 | 10.250.19.102 | 10.251.215.16 | 1 | cross-subnet (inter-rack) |
| 11 | 10.250.18.114 | 10.251.39.179 | 1 | cross-subnet (inter-rack) |
| 12 | 10.251.197.226 | 10.251.199.225 | 1 | same subnet |
| 13 | 10.251.197.226 | 10.251.30.85 | 1 | same subnet |
| 14 | 10.250.10.6 | 10.250.18.114 | 1 | same subnet |
| 15 | 10.250.10.6 | 10.251.203.149 | 1 | cross-subnet (inter-rack) |

### Top Directed Pairs (src → dst)
| Rank | Source | Destination | # Directed Events |
|---|---|---|---|
| 1 | 10.250.14.224 | 10.251.215.16 | 3 |
| 2 | 10.251.215.16 | 10.251.74.79 | 3 |
| 3 | 10.251.107.19 | 10.251.31.5 | 3 |
| 4 | 10.251.31.5 | 10.251.90.64 | 3 |
| 5 | 10.250.14.224 | 10.251.71.193 | 2 |
| 6 | 10.251.215.16 | 10.251.107.19 | 2 |
| 7 | 10.251.107.19 | 10.251.71.240 | 2 |
| 8 | 10.250.11.100 | 10.250.19.102 | 1 |
| 9 | 10.251.111.209 | 10.250.19.102 | 1 |
| 10 | 10.251.215.16 | 10.250.19.102 | 1 |
| 11 | 10.251.39.179 | 10.250.18.114 | 1 |
| 12 | 10.251.197.226 | 10.251.199.225 | 1 |
| 13 | 10.251.197.226 | 10.251.30.85 | 1 |
| 14 | 10.250.10.6 | 10.250.18.114 | 1 |
| 15 | 10.250.10.6 | 10.251.203.149 | 1 |

### Traffic Characterisation
- **Same-subnet (intra-rack) interactions:** 255 (62.2% of bilateral events)
- **Cross-subnet (inter-rack) interactions:** 155 (37.8%)
- Same-subnet traffic dominates, consistent with data-local reads/map tasks and the local write pipeline's first replica.

---
## 5. DataNode vs FSNamesystem Activity
The log mixes messages from three component classes — `dfs.DataNode` (including sub-components `DataXceiver`, `DataTransfer`, `PacketResponder`) and `dfs.FSNamesystem`.
| Component | Log Lines | % of Total | Primary Responsibility |
|---|---|---|---|
| **DataNode** (DataXceiver + DataTransfer + PacketResponder + base DataNode) | 1592 | 79.6% | Block receive/serve, pipeline transfer, packet acknowledgement, replication push |
| **FSNamesystem** (NameNode) | 408 | 20.4% | Block allocation (`allocateBlock`), block-map updates (`addStoredBlock`), replication scheduling (`ask ... to replicate ...`) |
| Other / Unclassified | 0 | 0.0% | — |

### Activity by Sub-component (DataNode side)
| DataNode Sub-component | Lines |
|---|---|
| DataXceiver (receives/serves blocks) | 1560 |
| PacketResponder (acks pipeline) | 24 |
| DataTransfer (transmits replicas) | 4 |
| DataNode (core/orchestration) | 4 |

### Activity by Operation Type (FSNamesystem side)
| FSNamesystem Operation | Lines |
|---|---|
| allocateBlock (new block for write) | 385 |
| addStoredBlock (register replica on DN) | 19 |
| replicate command (push replica) | 4 |

### How the Two Components Interact
1. A client requests a new block → **FSNamesystem.allocateBlock** picks pipeline targets.
2. The client streams data → target **DataNode$DataXceiver** logs `Receiving block ... src:/client dest:/self`.
3. Each DN in the pipeline forwards to the next (cross-subnet transfers show as `DataTransfer: Transmitted block ... to /<next-dn>`).
4. After the packet pipeline completes, **PacketResponder** logs acks and `Received block ... of size N from /<upstream>`.
5. Each DN reports the final replica to the NN → **FSNamesystem.addStoredBlock** updates the block map.
6. When a block is under-replicated, **FSNamesystem** issues `ask <dn> to replicate ... to datanode(s) ...`, triggering the source DN to start a transfer thread (`DataNode: Starting thread to transfer block ... to <targets>`).

---
## 6. Cluster Size Estimate
- **Unique IPs observed in this log:** **202**
- **IPs that appear as DataNodes (identified by `:50010` DataNode service port in src/dest lines, `Served block`, `Transmitted block`, `addStoredBlock`, or replication targets):** **202** (effectively all unique IPs)
- **IPs explicitly bound to the DataNode service port (50010) or registered via `addStoredBlock`:** **202**

### Estimate
The log slice captures traffic involving approximately **202 DataNodes** across two /16 subnets. Because the log is a *single* DataNode's log (2,000 lines), it records only the nodes that this DN directly exchanged blocks with during the window, plus any nodes referenced by NameNode replication commands. The total HDFS cluster is therefore **at least 202 DataNodes**, and likely somewhat larger (nodes with which this DN did not communicate during the trace will not appear).

Breakdown by subnet:
- `10.250.x.x`: ≈ **38** DataNodes observed
- `10.251.x.x`: ≈ **164** DataNodes observed

NameNode(s) are **not explicitly visible by IP** in this excerpt (FSNamesystem messages are logged locally and don't carry a remote NN address). The NameNode is co-located with this log's host or reachable via a configured address not emitted in these messages.

---
## Appendix: Parsing Methodology
- Extracted IPv4 addresses using regex; distinguished source vs destination roles via context-specific patterns (`src:/… dest:/…`, `Served … to /…`, `Transmitted … to /…`, `Received … from /…`, `Starting thread to transfer … to …`, `ask … to replicate … to datanode(s) …`, `blockMap updated: … is added`, `Opening connection to …`).
- Subnet grouping uses `/16` (first two octets) to match the explicit `10.250.x.x` / `10.251.x.x` pattern in the prompt and to mirror Hadoop's typical rack-level subnet split.
- Pair interactions are counted once per directed event; undirected pair counts normalize direction for ranking.
- Component attribution uses the logger name (`dfs.DataNode$DataXceiver`, `dfs.DataNode$PacketResponder`, `dfs.DataNode$DataTransfer`, `dfs.DataNode`, `dfs.FSNamesystem`).
- Cluster-size estimate is a **lower bound**: it counts nodes that this DataNode (the one writing this log) spoke with during the sampled time window.
