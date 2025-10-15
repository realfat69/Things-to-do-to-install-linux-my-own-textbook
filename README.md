# Things-to-do-to-install-linux-my-own-textbook
This respository shows how to mount disk, create boot partition, dual boot from grub





Khi ở trong cachyOS iso thì phân ra 2 vùng chính
1 là 1 vùng 512MiB với định dạng fat32, gắn tag là boot là mount point sẽ là /boot/efi
2 là vùng dữ liệu với định dạng ext4 và mount point là /


sau khi cài thành công thì thiết lập grub

vào Bios và chọn boot option là hệ điều hành vừa cài ( lưu ý phải tắt secure boot, nếu lỡ chưa tắt thì ctrl alt del để restart rồi spam bios access button )

do sử dụng CachyOS là Arch-based nên làm các bước sau


```
sudo efibootmgr
```
Tìm cái grub boot cũ từ linux trước và xóa nó tận gốc nếu có 
nó kiểu sẽ ra như là 
```
Boot0000* Fedora   HD(1,GPT,...) /File(\EFI\fedora\shimx64.efi)
Boot0001* Windows Boot Manager   /File(\EFI\Microsoft\Boot\bootmgfw.efi0000424f)
Boot0003* CachyOS   /File(\EFI\CachyOS\grubx64.efi)
Boot0004* UEFI OS   /File(\EFI\BOOT\BOOTX64.EFI0000424f)
```

ví dụ 0000 là boot cũ thì
```
sudo efibootmgr -b 0000 -B
```

xóa tận gốc :
```
sudo mount /boot/efi
sudo rm -r /boot/efi/EFI/fedora
```

sau đó tạo boot mới để dualboot từ grub với windows và cachyOSboot
bước 1 : cho os-prober chạy
```
sudo nano /etc/default/grub
```

tìm dòng
```
#GRUB_DISABLE_OS_PROBER=false
```
bỏ dấu # sau đó ctrl o, enter, ctrl X


bước 2: sau đó tạo boot 

```
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

sau đó nó ghi kiểu như này là thành công :
```
Found Windows Boot Manager on /dev/nvme0n1p1@/EFI/Microsoft/Boot/bootmgfw.efi
```


reboot
