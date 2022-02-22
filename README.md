# ld_preload-rsession

* change `upper` and `lower` in `shim_bind.c` for your local firewall settings 
* compile `shim_bind.c` via `gcc -Wall -O2 -fpic -shared -ldl -o shim_bind.so shim_bind.c`
* copy `shim_bind.so` to a shared location 
* edit `rsession-profile` to make sure `LD_PRELOAD` points to the actual path of `shim_bind.so` 
* add `rsession-profile` to your RStudio configuration files in `/etc/rstudio` 


Note: If you would like to use another directory than `/etc/rstudio`, please consider the approach described in [the Admin Guide](https://docs.rstudio.com/ide/server-pro/server_management/core_administrative_tasks.html#alternate-configuration-file-location) 
