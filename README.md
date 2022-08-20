# LibRerand Beta Test
Platform: x86_64 \
OS: Ubuntu 18.04

## Quick test Ubuntu 18.04
1. Add install source: ```curl -s https://packagecloud.io/install/repositories/ACD/librerand/script.deb.sh | sudo bash```
2. Run command to install librerand: ```sudo apt-get install librerand``` 
3. Set environment variables: ```export RERAND=2; export LD_BIND_NOW=1``` 
4. Run sample application compiled with librerand: ```sample-target``` 
5. Check log file with prefix phalanx in /tmp/Logs directory, it contains timestamps of randomizations

### Run with your own apps
Note: intended to be used with compiled executables
1. Set environment variables: ```export RERAND=2; export LD_BIND_NOW=1``` 
2. Run your app with librerand: ```LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libRereand.so ./myapp``` \
    2.1 An example run: ```LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libRereand.so redis-server```

