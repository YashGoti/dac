#!/bin/bash

function log(){
        echo -e $1
}

function help(){
        log "Flags:"
        log "\t-h\tshow help menu"
        log "\t-d DOMAIN | --domain DOMAIN\tfor specify domain"
        log "\t-da | --domaintoasn\t\tdomain to asn number"
        log "\t-dc | --domaintocidr\t\tdomain to cidr"
        log "\t-ac ASN   | --asntocidr ASN\tasn number to cidr"
        log "\t\t\t\t\tEx.8911,50313 OR 8911"
}

function domaintoasn(){
        domain=$1
        org=$(echo $domain | cut -d '.' -f1)
        ip=$(host $domain | grep 'has address' | awk '{print $NF}')
        curl -s "https://ipinfo.io/$ip/org" | awk '{print $1}'| sed 's/^AS/AS /g' | awk '{print $NF}' | sort -u
}

function domaintocidr(){
        domain=$1
        org=$(echo $domain | cut -d '.' -f1)
        ip=$(host $domain | grep 'has address' | awk '{print $NF}')
        asn=$(curl -s "https://ipinfo.io/$ip/org" | awk '{print $1}')
        curl -s "https://ipinfo.io/$asn" | grep -Eo '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\/[0-9]{1,3}' | sort -u
}

function asntocidr(){
        asn=$1
        if [[ $asn =~ , ]];then
                for number in $(echo -e $asn | sed 's/\,/\n/g')
                do
                        curl -s "https://ipinfo.io/AS$number" | grep -Eo '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\/[0-9]{1,3}' | sort -u
                done
        else
                curl -s "https://ipinfo.io/AS$asn" | grep -Eo '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\/[0-9]{1,3}' | sort -u
        fi
}

if [[ -z $@ ]];then
        log "[WAN] Try -h | --help for help menu"
else
        while [[ $# -gt 0 ]]; do
                flag=$1
                shift

                case $flag in
                        -h | --help )
                                help
                                shift
                        ;;
                        -d | --domain )
                                if [[ -z $1 ]]; then log "[ERR] -d | --domain required value"; else domain=$1; fi
                                shift
                        ;;
                        -da | --domaintoasn )
                                domaintoasn $domain
                                shift
                        ;;
                        -dc | --domaintocidr )
                                domaintocidr $domain
                                shift
                        ;;
                        -ac | --asntocidr )
                                if [[ -z $1 ]]; then log "[ERR] -ac | --asntocidr required asn numbers" log "Format 8911,50313,394161 OR 8911"; else asn=$1; fi
                                asntocidr $asn
                                shift
                        ;;
                        * )
                                help
                                shift
                        ;;
                esac

        done
fi
