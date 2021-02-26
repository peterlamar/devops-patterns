# FluentD

FluentD is a useful tool that will pull logs from a variety of sources and output them in a variety of formats including json, regular logs, etc. 

## Tutorial

```
curl -X POST -d 'json={"json":"message"}' http://localhost:8888/debug.test
tail /var/log/td-agent/td-agent.log
```

## Example

This would parse the syslog and pull only kernel messages, based on the [fluentD](https://docs.fluentd.org/how-to-guides/parse-syslog) docs. 

1.

```bash
sudo vim /etc/td-agent/td-agent.conf
```

2. 

```
<source>
  @type syslog
  port 5140
  tag system
</source>
 
<filter system.**>
  @type grep
  <regexp>
    key ident
    pattern /^kernel$/
  </regexp>
</filter>
 
<match system.**>
  @type stdout
</match>
```

3. 

```
sudo systemctl restart td-agent
```

4. 

```
less /var/log/td-agent/td-agent.log
```

Sample Log
```
2021-02-26 13:15:10.000000000 -0500 system.kern.info: {"host":"puppetself","ident":"kernel","message":"[   32.152959] usb 1-1: USB disconnect, device number 2"}
```