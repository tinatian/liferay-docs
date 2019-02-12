# Choosing IPv4 or IPv6 [](id=choosing-ipv4-or-ipv6)

@product@ supports both the IPv4 and IPv6 address formats. By default, it uses
IPv4 addresses. If you're on an IPv6 network, you must change the configuration.
If you'd like more information on the basics of these protocols, you can check
out the
[reason](http://www.google.com/intl/en/ipv6/)
for using IPv6 addresses, and its
[technical details](http://en.wikipedia.org/wiki/IPv6).

To configure your portal to validate IPv6 addresses, complete these steps:

1. If you're using @product@ on the Tomcat app server, for example, edit the
   `setenv.sh` or `setenv.bat` file in the `${TOMCAT_HOME}/bin` folder and set
   `-Djava.net.preferIPv4Stack=false` in `CATALINA_OPTS`.
2. Create a `portal-ext.properties` file in your portal's
   [Liferay Home folder](/discover/deployment/-/knowledge_base/7-1/installing-liferay#liferay-home)
   (if one does not already exist) and set the `tunnel.servlet.hosts.allowed`
   property to the target hosts you want to allow (e.g., *0:0:0:0:0:0:0:1*).
