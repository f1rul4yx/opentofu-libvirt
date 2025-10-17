# Configuración

- Se modifican las variables.tf para poner los datos del pool
- Se modifica cloud-init/server1/user-data.yaml para configurar los usuarios de las máquinas
- Se modifica main.tf para configurar imagen base, name, memory, cpu
- En cloud-init.tf une los ficheros de configuración

## Configuración apparmor

```bash
sudo nano /etc/apparmor.d/libvirt/TEMPLATE.qemu
```

```
#
# This profile is for the domain whose UUID matches this file.
#

#include <tunables/global>

profile LIBVIRT_TEMPLATE flags=(attach_disconnected) {
  #include <abstractions/libvirt-qemu>
  /var/lib/libvirt/images/*.qcow2 rwk,
}
```

```bash
sudo systemctl reload apparmor.service
sudo systemctl restart libvirtd
```

# Ejecución

```bash
tofu init
```

> Se descarga el plugin del provider teniendo en cuenta el fichero provider.tf

```bash
tofu plan
```

> Muestra el plan que se va a realizar

```bash
tofu apply
```

> Intenta llegar al estado modificado (crear todo)

```bash
tofu destroy
```

> Elimina lo creado
