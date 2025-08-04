

---

sudo mkdir /opt/ltp_results/syscalls-ipc

sudo ./kirk -f syscalls-ipc \
  -U ltp \
  -o /opt/ltp_results/syscalls-ipc/report.json \
  --suite-timeout 10800 \
  -v
