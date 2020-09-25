# Vorbereitung für Paketierung

1. Download des aktuellen [Agenten für Mac OSX](https://github.com/OCSInventory-NG/UnixAgent/releases/download/v2.6.0-MAC/Ocsinventory_Agent_MacOS-2.6.0.pkg.zip)
2. Paketinhalt anzeigen lassen und die Datei `Contents/Packages/OCS Inventory Pkg Setup.pkg` lokal kopieren.
3. Von dieser extrahierten Datei die Paketinhalte anzeigen lassen
4. Die extrahierte Datei über `pkgutil --expand "OCS Inventory Pkg Setup.pkg" expanded entpacken
5. Die Datei `expanded/Scripts/postinstall` bearbeiten
   1. Ersetze `launchctl load $LAUNCHDPATH/org.ocsng.agent.plist` durch `launchctl load -w $LAUNCHDPATH/org.ocsng.agent.plist`
   2. Lösche das Starten des Dienstes via `launchctl start org.ocsng.agent`
   3. Folgender Block muss oberhalb des Ablegens der LaunchDaemon Datei eingefügt werden:
      if [ -f $TMP_DIR/ocs-agent-start.sh ]; then
      	cp $TMP_DIR/ocs-agent-start.sh /usr/local/sbin
      	chown root:wheel /usr/local/sbin/ocs-agent-start.sh
      	chmod 744 /usr/local/sbin/ocs-agent-start.sh
      fi
6. Das Paket über `pkgutil --flatten expanded "OCS Inventory Pkg Setup.pkg"` wieder zusammenbauen.
7. Das neu erstellte Paket wieder in den Paketinhalt unter `Contents/Packages/OCS Inventory Pkg Setup.pkg` ablegen