# dac [DOMAIN, ASN, CIDR]
Fetch ASN [Number] / CIDR [IP Range] from Domain, Fetch CIDR [IP Range] from ASN [Number] using https://ipinfo.io/ API

### Installation
```
git clone https://github.com/YashGoti/dsc.git
cd dac
chmod +x dac
```

### Usage
```
./dac -d domain.com -da
./dac -d domain.com -dc
./dac -ac 8911,50313 OR ./dac -ac 8911
```

### Further Usage
```
for asn in $(./dac -d domain -da | xargs | sed 's/ //g' | sed 's/,$//')
do
  amass intel -asn $asn
done
```
```
for iprange in $(./dac -d domain -dc)
do
  amass intel -cidr $iprange
done
```
