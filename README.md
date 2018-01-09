1. After building dragonglass, Copy the dragonglass folder and paste into the Dragonglasswallet folder

2. Build wallet

```
mkdir build && cd build && cmake .. && make
```



3. After wallet is built, open "Dragonglasswallet" in build/src

copy address of new wallet
close Dragonglasswallet


4. go to  /dragonglass/build/release/src in terminal

./dragonglassd --start_mining *paste your address [enter]


reopen DragonglassWallet
