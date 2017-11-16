**Because of use of specific qt 5.6 and higher functions this guide will only work on ubuntu 17.10, u may have success on lower versions 
if u manual install qt 5.9 or higher.**

Start on ubuntu your terminal (search for terminal with ubuntu dash bottom icon left, or press windows key to open dash).
First we need to install the dependencies, for this execute the following cmd’s stated below in the terminal window.

The commands to execute are in the grey textboxes. 

The commands will ask for a password, and u have to confirm some commands with y from yes.

First update the apt library.

```
sudo apt-get update
```

Then we need to install the dependencies to be able to compile Zoin.


```
sudo apt-get install libqt5gui5 libqt5core5a libqt5dbus5 qttools5-dev qttools5-dev-tools git build-essential \
libssl-dev libboost-system-dev libboost-filesystem-dev libboost-program-options-dev libboost-thread-dev \
libssl-dev libminiupnpc-dev libqrencode-dev -y
```
Next we need to install the recommended Berkeley db-4.8.30.NC

```
sudo add-apt-repository ppa:bitcoin/bitcoin
```
```
sudo apt-get update
```
```
sudo apt-get install libdb4.8-dev libdb4.8++-dev -y
```

Now we are ready to compile zoin execute the following commands.

```
git clone  https://github.com/zoinofficial/zoin
```
```
cd zoin
```
```
qmake -qt=qt5
```
```
make
```
```
After some time the zoin-qt file will be in the zoin dir in your home folder.
