# openvpn-scripts-zvC9
Amateur scripts to setup openvpn with generation of keys (ED25519), CSRs, CERTs, configs and dhparam(s).

These scripts and usage instruction were created by an amateur. Don't use them until you check that they are suitable and secure.

ed25519 keys are used

You have 3 machines: CA, client, server.
Can be CA=server or CA=client.
Can NOT be client=server.

Note: where you are going to type "${netname}", type value of variable "netname", which
you have set in "bashlib/openvpn-openssl-vars.bash". If it's set to "mytestnet1",
string "${netname}" becomes "mytestnet1".

What to do:
- Edit "bashlib/openvpn-openssl-vars.bash"
- Copy whole project directory "openvpn-scripts-zvC9" with edited
  "openvpn-scripts-zvC9/bashlib/openvpn-openssl-vars.bash"
  to client, server, CA.
- On client and on server launch  "generate-openvpn-configs.bash"
- On client launch "client-generate-openvpn-client-key-and-CSR.bash"
- On server launch "server-generate-openvpn-server-key-and-CSR-and-dhparam.bash"
- On CA, launch "CA-generate-openvpn-CAkey.bash"
- copy from client "created_files_for_use/client/generated/${netname}/${netname}-tls-client.csr.pem" into same directory on CA
- copy from server "created_files_for_use/server/generated/${netname}/${netname}-tls-server.csr.pem" into same directory on CA
- launch "CA-sign-CSRs.bash" on CA
- from CA copy file "created_files_for_use/client/generated/${netname}/${netname}-tls-client.ca-signed.cert.pem" to same directory on client
- from CA copy file "created_files_for_use/server/generated/${netname}/${netname}-tls-server.ca-signed.cert.pem" to same directory on server
- On server, copy "created_files_for_use/server/generated/${netname}" into "/etc/openvpn/generated/"
- On server, copy "created_files_for_use/server/${netname}-tls-server.conf" into "/etc/openvpn/"
- On client, copy "created_files_for_use/client/generated/${netname}" into "/etc/openvpn/generated/"
- On client, copy "created_files_for_use/client/${netname}-tls-client.conf" into "/etc/openvpn/"
- On server, if you are using systemd init system, run in bash as root "systemctl enable openvpn@${netname}-tls-server",
  where var "netname" has value which you configured in "bashlib/openvpn-openssl-vars.bas"
Note: you can run "source bashlib/openvpn-openssl-vars.bas" in bash to get variables set
- On client, if you are using systemd init system, run in bash as root "systemctl enable openvpn@${netname}-tls-client",
  where var "netname" has value which you configured in "bashlib/openvpn-openssl-vars.bas"
Note: you can run "source bashlib/openvpn-openssl-vars.bas" in bash to get variables set
- If you are using non-systemd init system, find other way to configure openvpn to start on boot.
- Reboot to launch openvpn or use terminal to launch it (with systemd can do this: "systemctl start openvpn@${netname}-tls-server" on server,
  "systemctl start openvpn@${netname}-tls-client" on client. Before running set "netname" variable as it is set in "bashlib/openvpn-openssl-vars.bash")


Note: can manually launch openvpn this way:
  server: "openvpn --config /etc/openvpn/${netname}-tls-server.conf",
  client: "openvpn --config /etc/openvpn/${netname}-tls-client.conf",
 don't forget to set "netname" variable as it is set in "bashlib/openvpn-openssl-vars.bash"
