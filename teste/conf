### Install Python, Pip and Virtualenv ###

sudo apt update
sudo apt install software-properties-common
sudo apt install python3 python3-pip - y
sudo apt install python3-dev python3-virtualenv python3-venv virtualenvwrapper -y

sudo -H pip3 install --upgrade pip

echo "export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3" | sudo tee --append ~/.bashrc
echo "export WORKON_HOME=~/.virtualenvs" | sudo tee --append ~/.bashrc
echo "export VIRTUALENVWRAPPER_VIRTUALENV=/usr/bin/virtualenv" | sudo tee --append ~/.bashrc
echo "source /usr/share/virtualenvwrapper/virtualenvwrapper.sh" | sudo tee --append ~/.bashrc

source ~/.bashrc
