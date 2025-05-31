## rofi_mullvad

This script assumes you downlaoded the mullvad wireguard profiles from their official website.  
Generate the wireguard profiles here: https://mullvad.net/en/account/wireguard-config?platform=linux  

Install all profiles in networkmanager:

    for FILE in /path/to/profiles/*.conf ; do nmcli connection import type wireguard file "$FILE" ; done

    # For some reason nmcli thinks it's a good idea to enable every 500+ imported profiles so we have to disable them all

    for FILE in $(nmcli connection show | grep wireguard | awk '{print $1}') ; do 
        nmcli connection down  $(basename $FILE|cut -d'.' -f1 )
    done

Now just run the script:

    $ ./rofi_mullvad
