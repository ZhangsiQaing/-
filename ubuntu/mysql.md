一、系统安装条件

1.cmake
sudo apt-get install cmake

2.bison
sudo apt-get install bison

3.ncurses
sudo apt-get install libncurses5-dev

5.Boost 1.59.0
wget https://sourceforge.net/projects/boost/files/boost/1.59.0/boost_1_59_0.tar.gz
tar -zxvf boost_1_59_0.tar.gz -C /usr/local/


二、编译MySQL
cmake . -DCMAKE_INSTALL_PREFIX=/opt/mysql -DMYSQL_DATADIR=/opt/mysqldata -DWITH_BOOST=/usr/local/boost_1_59_0 -DEXTRA_CHARSETS=all


三、