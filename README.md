Add 'dragonglass' folder into the Dragonglasswallet folder by copy paste


or

```
git submodule add https://github.com/ZirtysPerzys/dragonglass.git dragonglass
```



Building wallet

```
mkdir build && cd build && cmake .. && make
```



After wallet is built, "Dragonglasswallet" executable will be found in (build/src)

copy address of new wallet
close Dragonglasswallet


go to  /dragonglass/build/release/src in terminal

./dragonglassd --start_mining *paste your address [enter]


reopen DragonglassWallet
