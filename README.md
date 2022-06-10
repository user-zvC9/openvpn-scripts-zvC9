# openvpn-scripts-zvC9
Amateur scripts to setup openvpn with keys, CSRs, CERTs, configs generation

These scripts and usage instruction were created by an amateur. Don't use them until you check that they are suitable and secure.

ed25519 keys are used

You have 3 machines: CA, client, server.
Can be CA=server or CA=client.
Can NOT be client=server.

Note: where you are going to type "${netname}", type value of variable "netname", which
you have set in "bashlib/openvpn-openssl-vars.bash". If it's set to "mytestnet1",
string "${netname}" becomes "mytestnet1".

What to do:
0. Edit "bashlib/openvpn-openssl-vars.bash"
1. On client and on server launch  "generate-openvpn-configs.bash"
2. On client launch "client-generate-openvpn-client-key-and-CSR.bash"
3. On server launch "server-generate-openvpn-server-key-and-CSR-and-dhparam.bash"
4. On CA, launch "CA-generate-openvpn-CAkey.bash"
5. copy from client "created_files_for_use/client/generated/&#36;\{netname\}/\$\{netname\}-tls-client.csr.pem" into same directory on CA
6. copy from server "created_files_for_use/server/generated/\${netname}/${netname}-tls-server.csr.pem" into same directory on CA
7. launch "CA-sign-CSRs.bash" on CA
8. from CA copy file "created_files_for_use/client/generated/${netname}/${netname}-tls-client.ca-signed.cert.pem" to same directory on client
9. from CA copy file "created_files_for_use/server/generated/${netname}/${netname}-tls-server.ca-signed.cert.pem" to same directory on server
10. On server, copy "created_files_for_use/server/generated/${netname}" into "/etc/openvpn/generated/"
11. On server, copy "created_files_for_use/server/${netname}-tls-server.conf" into "/etc/openvpn/"
12. On client, copy "created_files_for_use/client/generated/${netname}" into "/etc/openvpn/generated/"
13. On client, copy "created_files_for_use/client/${netname}-tls-client.conf" into "/etc/openvpn/"
14. On server, if you are using systemd init system, run in bash as root "systemctl enable openvpn@${netname}-tls-server",
  where var "netname" has value which you configured in "bashlib/openvpn-openssl-vars.bas"
Note: you can run "source bashlib/openvpn-openssl-vars.bas" in bash to get variables set
15. On client, if you are using systemd init system, run in bash as root "systemctl enable openvpn@${netname}-tls-client",
  where var "netname" has value which you configured in "bashlib/openvpn-openssl-vars.bas"
Note: you can run "source bashlib/openvpn-openssl-vars.bas" in bash to get variables set
16. If you are using non-systemd init system, find other way to configure openvpn to start on boot.
17. Reboot to launch openvpn or use terminal to launch it (with systemd can do this: "systemctl start openvpn@${netname}-tls-server" on server,
  "systemctl start openvpn@${netname}-tls-client" on client. Before running set "netname" variable as it is set in "bashlib/openvpn-openssl-vars.bash")

Note: can manually launch openvpn this way:
  server: "openvpn --config /etc/openvpn/${netname}-tls-server.conf",
  client: "openvpn --config /etc/openvpn/${netname}-tls-client.conf",
 don't forget to set "netname" variable as it is set in "bashlib/openvpn-openssl-vars.bash"
 
