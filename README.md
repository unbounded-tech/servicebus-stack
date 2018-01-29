# registry stack

before forking, create the following environment variables in Jenkins:

## `rmq1Domain`
Example: `rmq1.yourdomain.com`

Also use credentials to create the following secret variables:

Type "Secret Text"
## `erlangCookie`
Random string used for clustering
Example: `asldjfasdf123nasd24`

Type "Username/Password
## `rmqPassword`
Default Password

## `rmqUser`
Default User

---

Note: When adding a stack that uses a new domain name, it's recommened to configure the DNS before
deploying to avoid DNS caching issues on your local machine. This occurs when you attempt
to go to a domain before the DNS for it has been configured. The unconfigured resolution will
can get cached on your router or computer, which technically doesn't cause issues for anyone
but you, but it is annoying!
