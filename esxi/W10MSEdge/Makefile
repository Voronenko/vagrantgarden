init:
	mkdir -p ./terraform.d/plugins/linux_amd64
	wget -O ./terraform.d/plugins/linux_amd64/terraform-provider-esxi "https://github.com/Voronenko/terraform-provider-esxi/releases/download/v1.5.2.bis/terraform-provider-esxi_v1.5.2_bis.2"
	chmod +x ./terraform.d/plugins/linux_amd64/terraform-provider-esxi
	terraform init

ovf-inspect:
	ovftool --hideEula /home/slavko/personal/ESXI/W10MSEdge/W10MSEdge.ova

check:
	ansible windows -i hosts -m win_ping

