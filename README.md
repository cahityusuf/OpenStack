# OpenStack

vagrant up --provider virtualbox

# VM

cd /openstack_deploy
git clone https://github.com/openstack/kolla-ansible.git .

source local/bin/activate

Using OpenStack
pip install python-openstackclient -c https://releases.openstack.org/constraints/upper/master
kolla-ansible post-deploy

openstack --os-cloud kolla-admin hypervisor list

# Kurulum

Install dependencies

sudo apt update
sudo apt install git python3-dev libffi-dev gcc libssl-dev libdbus-glib-1-dev

Install dependencies for the virtual environment

sudo apt install python3-venv
python3 -m venv /path/to/venv
source /path/to/venv/bin/activate
pip install -U pip

Install Kolla-ansible

pip install git+https://opendev.org/openstack/kolla-ansible@master
sudo mkdir -p /etc/kolla
sudo chown $USER:$USER /etc/kolla
cp -r /path/to/venv/share/kolla-ansible/etc_examples/kolla/* /etc/kolla
cp /path/to/venv/share/kolla-ansible/ansible/inventory/all-in-one .
cp /path/to/venv/share/kolla-ansible/ansible/inventory/multinode .

Install Ansible Galaxy requirements

kolla-ansible install-deps

Prepare initial configuration

cd /etc/kolla
kolla-genpwd

# all-in-one
Kolla globals.yml

kolla_base_distro: "ubuntu"
network_interface: "eth0"
neutron_external_interface: "eth1"
kolla_internal_vip_address: "10.0.2.250"
enable_haproxy: "no"

Deployment

kolla-ansible bootstrap-servers -i ./all-in-one
kolla-ansible prechecks -i ./all-in-one
kolla-ansible deploy -i ./all-in-one

# multinode

docker run -d -p 5000:5000 --restart always --name registry registry:2
vim globals.yml
docker_registry: 192.168.1.100:4000
docker_registry_insecure: yes

vim /etc/hosts
192.168.60.3 control01 control02 control03
192.168.60.5 network01 network02
192.168.60.6 compute01
192.168.60.7 monitoring01 storage01
192.168.60.8 torage01

ssh-keygen -t rsa -b 4096 -C "cahityusuf@gmail.com"
ssh-copy-id vagrant@control01
ssh-copy-id vagrant@control02
ssh-copy-id vagrant@control03
ssh-copy-id vagrant@network01
ssh-copy-id vagrant@network02
ssh-copy-id vagrant@compute01
ssh-copy-id vagrant@storage01
ssh-copy-id vagrant@monitoring01

# control01, network01, compute01, monitoring01, storage01 => install app
sudo apt update
sudo apt install systemd-timesyncd
systemctl restart systemd-timesyncd
systemctl status systemd-timesyncd

sudo apt install chrony -y
sudo systemctl enable chrony
sudo systemctl start chrony
sudo systemctl status chrony
chronyc tracking

apt  install docker.io

apt install python3-pip
pip3 install docker
python3 -c "import docker; print(docker.__version__)"


# Tüm diğer sunucular için aynı komutu uygulayın.


# kolla-ansible bootstrap-servers -i ./all-in-one =>komutu hata çözümü

sudo apt-get update
sudo apt-get install -y libglib2.0-dev

pip3 install docker>=7.0.0 requests dbus-python


# Machine

git checkout -b local_dev_branch

# Jenkins

Jan 04 20:56:31 ubuntu2204.localdomain jenkins[67313]: e9538ab30a4a41c99c5fbb743d736755
Jan 04 20:56:31 ubuntu2204.localdomain jenkins[67313]: This may also be found at: /var/lib/jenkins/secrets/initialAdminPassword