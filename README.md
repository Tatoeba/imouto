# Imouto

### This repository has moved to https://github.com/Tatoeba/tatoeba2.

The code from this repository has been merged into the [main Tatoeba code base](https://github.com/Tatoeba/tatoeba2). "Imouto" has also been renamed to "Tatovm" to avoid confusion with this repository. If you already have an installation of Imouto, and you want to keep your local code modifications or your database, please check the guide below to migrate from Imouto to Tatovm. Otherwise, just check the [new instructions](https://github.com/Tatoeba/tatoeba2/blob/dev/README.tatovm.md) about how to run Tatovm.

### How to migrate from Imouto to Tatovm

The main difference between Imouto and Tatovm is that the source code now resides *outside* of the virtual machine. So in this guide we are going to get the code out of the Imouto VM and create a new Tatovm VM from it.

1. Make sure Imouto is up to date and start the Imouto VM.
    ```
    cd /your/path/of/imouto/
    git pull
    vagrant up
    ```
2. Copy the source code out.
    ```
    cp -ar Tatoeba/ ../new_tatoeba
    ```
3. Backup your database.
    ```
    vagrant ssh -c 'mysqldump --all-databases | gzip' -- -T > ../new_tatoeba/databases.sql.gz
    ```
4. Shut down the Imouto VM.
    ```
    vagrant halt
    ```
5. Go inside the directory you just copied.
    ```
    cd ../new_tatoeba/
    ```
6. Make sure your fork is up to date (if you have one).
    ```
    git fetch upstream
    git checkout dev
    git merge upstream/dev
    ```
7. If you are using Windows, make sure to turn off auto CRLF conversion.
    ```
    git config --global core.autocrlf false
    git checkout .
    ```
8. Setup and run the Tatovm VM. At the end, it should ask you if you want to restore your database backup. Just press enter to restore.
    ```
    vagrant up
    ```
9. Make sure the new VM is working correctly (go http://localhost:8080/ etc.) If something’s wrong, you can open an issue on Github and we’ll try to assist you.

10. Follow the instructions below to uninstall Imouto.

### Uninstalling Imouto

Just removing the old Imouto directory won’t remove the old VM. Since the VM takes quite some disk space, you may want to remove it:
```
cd /your/path/of/imouto/
vagrant destroy
```

Alternatively, you can start VirtualBox and remove the VM from there using right-click → Remove… → Delete all files.
