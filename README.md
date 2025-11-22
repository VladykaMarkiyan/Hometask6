Vladyka Markiyan 4CS-31

```
vagrant up                                запуск машинки
vagrant destroy -f                        видалити машину
vagrant reload                            перезапуск машини
vagrant ssh                               увійти в ssh
pvcreate /dev/sdX1                        створення LVM
vgcreate vg_data /dev/sd[b-e]1            створення volume group
lvcreate -L 800M -n lv1 vg_data           логічні томи
```