# terraform

### Установка terraform 
```
wget https://releases.hashicorp.com/terraform/1.6.6/terraform_1.6.6_linux_amd64.zip
```
```
wget https://hashicorp-releases.yandexcloud.net/terraform/1.9.5/terraform_1.9.5_linux_amd64.zip
```
```
sudo apt install unzip
unzip terraform_1.6.6_linux_amd64.zip
sudo mv terraform /usr/local/bin/
```
```
terraform -v
```
```
terraform -install-autocomplete
```

### Provider 
```
git clone https://github.com/Telmate/terraform-provider-proxmox
```
```
cd terraform-provider-proxmox
```
#### GO

```
wget https://go.dev/dl/go1.23.0.linux-amd64.tar.gz
```
```
rm -rf /usr/local/go && tar -C /usr/local -xzf go1.23.0.linux-amd64.tar.gz
export PATH=$PATH:/usr/local/go/bin
```
```
go version
```

#### GCC

```
sudo apt install gcc make-guile
```

Then to compile the provider:

```
make
```
The executable will be in the ./bin directory.

```
wget https://github.com/Telmate/terraform-provider-proxmox/releases/download/v3.0.1-rc4/terraform-provider-proxmox_3.0.1-rc4_linux_amd64.zip
```
```
unzip terraform-provider-proxmox_3.0.1-rc4_linux_amd64.zip
```
```
mv terraform-provider-proxmox_3.0.1-rc4_linux_amd64 bin/terraform-provider-proxmox
```

```
mkdir -p ~/.terraform.d/plugins/registry.terraform.io/telmate/proxmox/3.0.1/linux_amd64/
```
```
cp bin/terraform-provider-proxmox ~/.terraform.d/plugins/registry.terraform.io/telmate/proxmox/3.0.1/linux_amd64/
```

```
cd ~/terraform
```
```
terraform init
```
