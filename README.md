## rofi_mullvad

This script assumes you downloaded the mullvad wireguard profiles from their official website.  
Generate the wireguard profiles here: https://mullvad.net/en/account/wireguard-config?platform=linux  

Install all profiles in networkmanager:

    # All 500+ profiles are set to autoconnect on import by default so we need to disable them
    for FILE in /path/to/profiles/*.conf ; do
        nmcli connection import type wireguard file "$FILE"
        nmcli connection modify "$(basename $FILE|cut -d'.' -f1)" +connection.autoconnect "no"
        echo "Disabled autoconnect for: $FILE"
        nmcli connection down "$(basename $FILE|cut -d'.' -f1)"
        echo "\n"
    done

Delete all (not only the just imported) wireguard profiles

    for PROFILE in $(nmcli connection show | grep wireguard | awk '{print $1}') ; do 
        nmcli connection delete "$PROFILE"
    done

Now just run the script:

    $ ./rofi_mullvad
