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

Install Ansible Galaxy requirements

kolla-ansible install-deps

Prepare initial configuration

cd /etc/kolla
kolla-genpwd

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

# kolla-ansible bootstrap-servers -i ./all-in-one =>komutu hata çözümü

sudo apt-get update
sudo apt-get install -y libglib2.0-dev

pip3 install docker>=7.0.0 requests dbus-python


# Machine

git checkout -b local_dev_branch

# Jenkins

Jan 04 20:56:31 ubuntu2204.localdomain jenkins[67313]: e9538ab30a4a41c99c5fbb743d736755
Jan 04 20:56:31 ubuntu2204.localdomain jenkins[67313]: This may also be found at: /var/lib/jenkins/secrets/initialAdminPassword