# ld_preload-rsession

* change `upper` and `lower` in `shim_bind.c` for your local firewall settings 
* compile `shim_bind.c` via `gcc -Wall -O2 -fpic -shared -o shim_bind.so shim_bind.c -ldl`
* copy `shim_bind.so` to a shared location 
* edit `rsession-profile` to make sure `LD_PRELOAD` points to the actual path of `shim_bind.so` 
* add `rsession-profile` to your RStudio configuration files in `/etc/rstudio` 

## Sanity checking

First, run in a terminal/shell session 
```
ldd /path/to/shim_bind.so
```
This should report 4 libraries, most importantly `libdl.so.2`.  

Furthermore you can check if port binding works by running  
```
LD_PRELOAD=/path/to/shim_bind.so nc -l -p 1234
```
This should make nc listen on port 1234. Since it is run against `LD_PRELOAD` it is forced to use a port from the port range, i.e. 59000...59999 in the example. 
Within RStudio you can check if LD_PRELOAD works via 
```
Sys.getenv("LD_PRELOAD") 
```
Finally you can check the port used by running in an RStudio terminal 
```
netstat -Ainet | grep ESTABLISHED
```
where you should see one connection between the client note and the RSW server that uses port 59xxx on the client side. 

Note: If you would like to use another directory than `/etc/rstudio`, please consider the approach described in [the Admin Guide](https://docs.rstudio.com/ide/server-pro/server_management/core_administrative_tasks.html#alternate-configuration-file-location) 
