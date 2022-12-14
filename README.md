# LibRerand
Platform: x86_64 \
OS: Ubuntu 18.04


## HIPIC Testing

### Installation live demo
Use example container, and install libRerand. Run ldd to verify libRerand is in place, and its dependencies are satisfied.

```
docker run -it armscyberdefense/integ_bionic:1.0 /bin/bash
curl -s https://packagecloud.io/install/repositories/ACD/librerand/script.deb.sh | bash
apt-get install librerand
ldd /usr/lib/x86_64-linux-gnu/libRerand*
```

### Configuration live demo
Use example container, configure rerand, start redis-server with libRerand. Then, run redis-benchmark against redis-server to ensure libRerand does not break redis-server. Finally, print out the libRerand log to check the timestamps of the randomizations.

```
docker run -it armscyberdefense/integ_bionic:1.1 /bin/bash
export RERAND=2; export LD_BIND_NOW=1
LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libRerand.so redis-server &
redis-benchmark
redis_pid=$(ps -ef | awk '/[r]edis/{print $2}')
libRerand_log=$(ls /tmp/Logs | grep $redis_pid)
cat /tmp/Logs/$libRerand_log
```

### Reconfiguration live demo
Use example container, configure rerand, start redis-server with libRerand. Then, at runtime, turn off rerandomization, and verify. Finally, turn back on rerandomization but with new period, and verify.

```
docker run -it armscyberdefense/integ_bionic:1.1 /bin/bash
export RERAND=2; export LD_BIND_NOW=1
LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libRerand.so redis-server &

#wait 30 seconds, timestamps should be 10 seconds (default) apart
redis_pid=$(ps -ef | awk '/[r]edis/{print $2}')
libRerand_log=$(ls /tmp/Logs | grep $redis_pid)
cat /tmp/Logs/$libRerand_log

#turn off rerandomization
echo "-1" > /home/.phalanx_conf
cat /tmp/Logs/$libRerand_log
#wait 30 seconds, logs should not have changed
cat /tmp/Logs/$libRerand_log

#turn back on rerandomization, with 4 second period
echo "4" > /home/.phalanx_conf
#wait 60 seconds, newest logs should be 4 seconds apart
cat /tmp/Logs/$libRerand_log
```

---------------------------------------------------------------------------------------------------------------------------

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

