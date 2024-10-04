# CheatSheetsHackathonCOPA-2024

## Introducción

## Linux

Red Hat pone a disposición de las personas que quieran aprender sobre Linux un ambiente libre y seguro sobre el cual se pueda aprender y probar algunas caracteristicas de Red Hat Enterprise Linux, de forma gratuita, y al que pueden acceder por medio del siguiente [enlace](https://www.redhat.com/en/interactive-labs/red-hat-enterprise-linux-open-lab). Te recomendamos que utilices este recurso si eres nuevo en el mundo de linux y quieres aprender.

A continuación encontrarás una serie de recursos que esperamos que te sean de utilidad. Ten en cuenta que puedes encontrar mucha más información, al igual que libros, tutoriales, artículos acerca de RHEL (Red Hat Enterprise Linux), entre otras tecnologías, como Ansible, Kubernetes y mucho más en el [Portal de developers de Red Hat](developers.redhat.com), y todo esto de forma gratuita.

En las proximas secciones encontrarás algunos comandos que te pueden resultar de interés o utilidad, pero antes de ello, te compartimos este [Cheat Sheet](https://developers.redhat.com/cheat-sheets/red-hat-enterprise-linux-8) de RHEL 8, que esperamos también te resulte útil.

### Gestión de usuarios

Comandos de utilidad:

- useradd
- adduser
- userdel
- passwd
- id
- chage

Archivos:

- /etc/passwd
- /etc/shadow
- /etc/group

### Uso de manuales

Comandos de utilidad:

- man
- Opciones de comando con --help o -h
- Tab
- apropos

Archivos:

- /bin
- /sbin
- /usr/bin

Documentación:

- docs.redhat.com

### Gestión de paquetes

Comandos de utilidad:

- yum / dnf
  - install
  - remove
  - update
  - history
- rpm
  - `-qa`
  - `-ql`
- subscription-manager
  - list
  - repolist
  - register
  - unregister

### Gestión de servicios (Iniciar y apagar)

Comandos de utilidad:

- enab
  - start
  - stop
  - restart
  - status
  - enable
  - disable
  - list-units (--type=service)
  - list-units (--type=target)
  - is-active
  - is-enable
  - systemctl get-default
  - set-default <valor>

### bash script

Bourne Again Shell o bash (abreviatura), implementa una capacidad pára programar usando comandos de Shell, haciendo que algunas tareas de gestión y de administración sean más sencillas. Se usan archivos ejecutables, por lo que debes otorgar permisos de ejecución a tus archivos de bash script, usando el comando `chmod`.

Siempre llevan el mismo cabecero:

```bash
#!/usr/bin/bash
```

Un ejemplo puede ser:

```bash
#!/usr/bin/bash

# imprimir hello world para la hackaton
echo "hello world"
echo "ERROR: Houston, we have a problem." >&2
```

#### Variables

```bash
var=$(hostname -s); echo $var
```

Literales (No retorna el valor de la variable)

```bash
echo Your username variable is \$USER
```

#### Ciclos

```bash
for VARIABLE in LIST
do Commands with VARIABLE
done
```

Ejemplo:

```bash
for HOST in host1 host2 host3; do echo $HOST; done
```

Ejemplo usando un grupo de datos

```bash
for HOST in host{1,2,3}; do echo $HOST; done
```

#### Decisiones

Comparar números:

- `gt`= greater than
- `ge`= greater than or equal to
- `lt`= less than
- `le`= less than or equal to
- `eq`= equal to another number
- `ne`= Not equal to another number

Comparar Strings:

- `=` o `==` Comparar si dos cadenas de texto son iguales
- `!=` Comparar si dos cadenas de texto son diferentes
- `z` -> Validar si la longitud de un string es de cero
- `n` -> Validar si la longitud de un string es diferente de cero

Directorios y archivos:

- `-d` -> revisar si un directorio existe
- `-f` -> revisar si un archivo existe
- `-r` -> validar si el usuario tiene permiso de lectura

Ejemplos:

```bash
test 1 -gt 0 ; echo $?

test 0 -gt 1 ; echo $?

[[ 1 -eq 1 ]]; echo $?

[[ 1 -ne 1 ]]; echo $?

[[ 8 -gt 2 ]]; echo $?

[[ 2 -ge 2 ]]; echo $?

[[ 2 -lt 2 ]]; echo $?

[[ 1 -lt 2 ]]; echo $?

[[ abc = abc ]]; echo $?

[[ abc == def ]]; echo $?

[[ abc != def ]]; echo $?
```

**Bonus:**

`$?`se usa para obtener el codigo de salida del comando que se haya ejecutado inmediatamente antes de ejecutar este comando. Si el resultado es `0`, quiere decir que la ejecución fue exitosa. Cualquier otro número por fuera de eso es indicativo de que se presentó un error o una ejecución parcial.

#### Condicionales

```bash
if <CONDITION>; then
      <STATEMENT>
      ...
      <STATEMENT>
elif <CONDITION>; then
      <STATEMENT>
      ...
      <STATEMENT>
else
      <STATEMENT>
      ...
      <STATEMENT>
fi
```

### SELinux

Es un mecanismo de MAC (Mandatory Access Control), integrado al kernel. En su core implementa un sistema de etiquetas.

- A cada proceso, archivo y puerto de red dentro de SELinux se le asigna una etiqueta que delinea su contexto de seguridad.
- Estas etiquetas abarcan un tipo y una función, lo que permite a SELinux la capacidad de imponer controles de acceso meticulosamente. 
- Esta intrincada estructura de etiquetas comprende designaciones de usuario, rol, tipo y nivel, generalmente conocidas como los componentes de MLS (Multi Level Security protection).

Beneficios:

- Minimize Attack Surface
- Process Segregation
- Enhanced Application Security
- Precise Access Management

modos de SELinux

- `Enforcing`: En este modo, SELinux aplica activamente las políticas de seguridad definidas. Cualquier infracción desencadena una respuesta inmediata, como bloquear el acceso no autorizado o generar una alerta.
- `Permissive`: En modo permisivo, SELinux registra las infracciones mientras aplica las políticas y sin bloquearlas activamente. Este modo es útil para identificar brechas en las políticas antes de pasar a la aplicación total.
- `Disabled`: SELinux se apaga en modo deshabilitado y DAC se convierte en el principal mecanismo de control de acceso.

Comandos:

- sestatus
- getenforce

Directorios de interes:

- /etc/selinux/config

**Nota**: Tener en cuenta que el modo del SELinux se restablece cuando se reinicia la máquina, por lo cual se debe cambiar el estado en el archivo de SELinux. (Buscar la fila que diga algo similar a `SELINUX=permissive`)

### Modificar memoria del sistema

- https://www.arsys.es/blog/redimensionar-discos-duros-linux-no-reinicio
- https://www.redhat.com/sysadmin/resize-lvm-simple

## Ansible

### ¿Qué es Ansible?
Ansible es un motor de automatización de TI de código abierto que automatiza el aprovisionamiento, la gestión de la configuración, la implementación de aplicaciones, la orquestación y muchos otros procesos de TI. Es de uso gratuito y el proyecto se beneficia de la experiencia y la inteligencia de sus miles de contribuyentes.

Red Hat® Ansible Automation Platform combina más de una docena de proyectos upstream en una plataforma empresarial unificada y con seguridad reforzada para la automatización de misión crítica. Se basa en la base del proyecto de código abierto para crear una experiencia de automatización de un extremo a otro para equipos multifuncionales.

`https://www.ansible.com/`

En Ansible existe un **nodo de control** y uno o más **nodos manejados**

## Instalar Ansible
Necesito instalar en el  **nodo de control**:
  - Linux o
  - WSL (Windows Subsystem for Linux) o
  - MacOS

Instalar usando:
  - Herramientas de la plataforma. Ej: `sudo apt install ansible` o `sudo dnf install ansible-core`
  - pip (PyPI) `pip3 install ansible`

### ¿Qué Incluye?
- `ansible`
- `ansible-playbook`
- `ansible-inventory`
- `ansible-galaxy`
- La colección [ansible.builtin.*](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/index.html)

### YAML YAML Ain't Markup Language - El lenguaje de Ansible

Explicación de los tipos de datos
#### Escalares
- números: `1, 10, 3.1416, 2.5E10`
- strings:
  ```
  así
  "así"
  'así'
  incluyendo espacios dentro de las palabras
  ```
- booleano (verdadero/falso): `true, false`

#### Colecciones

- listas de cosas
```yaml
- uno
- dos
- 3
- true
```

- diccionarios/mappings/hash de cosas

```yaml
polar bears: polo norte
penguins: polo sur
neo: 1
trinity: 3
1: one
2: two
```

- puedo anidarlos y combinarlos

```yaml
albumes_shakira: # tienen un orden
  - Magia
  - Peligro
  - Pies descalzos
patos: # no tienen orden
  Hugo: sobrino
  Paco: sobrino
  Luis: sobrino
```

```yaml
# lista de objetos
- nombre: desayuno
  alimentos:
    - huevos
    - pan
    - jugo
  comi_en_casa: true
- nombre: almuerzo
  alimentos:
    - carne
    - arroz
    - plátano
  comi_en_casa: false
```

#### Referencias

YAML Parser online https://yaml-online-parser.appspot.com/
YAML Standard 1.2 https://yaml.org/spec/1.2.2/

### Playbooks - Estructura de un playbook

Los *playbooks* de Ansible son datos especiales en YAML/YML: una lista de *plays*.
Un *play* es un *dictionary/mapping/hash* especial. 

```yaml
- name: El nombre del playbook
  hosts: # una descripción de los nodos manejados
  become: # true o false, para decidir si correr todo como root
  vars: # variables que puedo agregar
  tasks:
    - name: Una tarea
      # ...
    - name: Segunda tarea
      # ...
```

### Módulos - la inteligencia de Ansible

Los módulos están escritos en Python e implementan la complejidad y la inteligencia de Ansible.

- ansible.builtin.debug
- ansible.builtin.command
- ansible.builtin.shell
- ansible.builtin.package

---
### Tareas
#### Estructura

Las tareas tienen:
- un nombre
- un módulo
- opcionalmente, parámetros para este módulo
- modificadores de la tarea (loops, condicionales, delegación, etc)

```yaml
- name: Nombre de la tarea
  # nombre del módulo
  ansible.builtin.user:
    # Parámetros
    user: minombredeusuario
    state: present
  # Al nivel de la tarea, modificadores
  delegate_to: localhost
  loop:
    - 1
    - 2
    - 3
  when: ambiente == 'development'
```

#### Variables
Las variables pueden venir de muchos lados:
- El inventario
- Declaradas explícitamente en un playbook
- Desde la llamada de `ansible_playbook`
- Variables "mágicas" que Ansible calcula por nosotros. Puedes averigüar lo que está disponible con el siguiente comando `ansible localhost -m setup`

https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_variables.html

Para usar su valor, usamos jinja con esta sintaxis:

```yaml
- name: Usamos variables por primera vez
  hosts: all
  # Definimos algunas variables para el playbook
  vars:
    ambiente: development
    un_numero: 100
  tasks:
    - name: Muestra las variables que definimos
      ansible.builtin.debug:
        msg: "Mi ambiente es {{ ambiente }} y el numero definido es {{ un_numero }}"

    - name: Muestra la version de S.O.
      ansible.builtin.debug:
        msg: "{{ ansible_distribution }}"

    - name: Puede ser un objeto complejo, como la info del tiempo
      ansible.builtin.debug:
        msg: "{{ ansible_date_time }}"
```

#### Registrar valores de tareas y usarlos luego

Ansible escribe lo mínimo necesario a pantalla, recuerda que podemos estar automatizando 20 o 100 equipos. Puedes grabar el resultado de una tarea y luego mostrarlo o tomar decisiones con esta información. https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_variables.html

```yaml
- name: Registrar valores y usarlos
  hosts: all
  tasks:
    - name: Averigua el tiempo que lleva corriendo el server
      ansible.builtin.command:
        cmd: uptime
      register: salida_uptime

    - name: Muestra el resultado de toda la tarea
      ansible.builtin.debug:
        msg: "{{ salida_uptime }}"

```
#### Loops y filtros

Podemos correr las mismas tareas muchas veces, haciendo loops de distintas formas. https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_loops.html

```yaml
- name: Podemos repetir la misma tarea para un conjunto de elementos
  hosts: all
  become: true
  tasks:
    - name: Crea un usuario
      ansible.builtin.user:
        name: ansible_user
        state: present

    - name: Crea varios usuarios
      ansible.builtin.user:
        name: "{{ item }}"
        state: present
      loop:
        - jose
        - maria
        - jesus

    - name: Usa un filtro para crear tu lista
      ansible.builtin.user:
        name: "user_{{ item }}"
        state: present
      loop: "{{ range(1, 10) }}"
```

Los filtros son parte de jinja y nos permiten transformar datos, generarlos, darles formato, etc.

- https://docs.ansible.com/ansible/2.7/user_guide/playbooks_filters.html
- https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_filters.html

### Playbooks - Tour de la colección builtin y módulos notables

- [copy module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/copy_module.html#ansible-collections-ansible-builtin-copy-module)
- [file module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/file_module.html#ansible-collections-ansible-builtin-file-module)
- [package module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/package_module.html)
- [lineinfile module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/lineinfile_module.html#ansible-collections-ansible-builtin-lineinfile-module)
- [service module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/service_module.html#ansible-collections-ansible-builtin-service-module)
- [uri module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/uri_module.html#ansible-collections-ansible-builtin-uri-module)
- [command module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/command_module.html#ansible-collections-ansible-builtin-command-module)
- [group module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/group_module.html#ansible-collections-ansible-builtin-group-module)
- [user module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/user_module.html#ansible-collections-ansible-builtin-user-module)

### Inventarios

Los archivos de inventario de Ansible son esenciales para definir los hosts (servidores o nodos) en los que se ejecutarán las tareas de automatización. Estos archivos contienen una lista de servidores y permiten agruparlos para aplicar configuraciones específicas o ejecutar playbooks sobre ellos de manera ordenada y organizada. Normalmente, se encuentra en /etc/ansible/hosts, aunque puedes usar cualquier otro archivo especificado con la opción -i cuando ejecutas un comando de Ansible. Los archivos de inventario pueden ser en formato `INI` o `YML`.

```ini
# Hosts individuales
server1 ansible_host=192.168.1.10
server2 ansible_host=192.168.1.20

# Grupo de hosts
# Agruoa los servidores web 1 y web2, en el grupo llamado "webservers"
[webservers]
web1 ansible_host=192.168.1.30
web2 ansible_host=192.168.1.40

[dbservers]
db1 ansible_host=192.168.1.50
db2 ansible_host=192.168.1.60

# Grupos anidados
# usando ":children" después del nombre del grupo, se especifica que
# se usarán los elementos contenidos en los grupos señalados, es
# decir los elementos de los grupos webservers y dbservers
[HackathonLabApp:children]
webservers
dbservers
```

Componentes clave de los archivos de inventario:
- Grupos: Puedes organizar hosts en grupos (como `webservers`, `dbservers`) para ejecutar tareas comunes en varios hosts al mismo tiempo.
- Variables: Se pueden definir variables específicas para cada host o grupo dentro del inventario, como el nombre de usuario para SSH, la dirección IP o el puerto SSH. Ejemplo en formato INI:

```ini
[webservers]
web1 ansible_host=192.168.1.30 ansible_user=root
web2 ansible_host=192.168.1.40 ansible_user=root
```

- Hosts dinámicos: Los archivos de inventario pueden ser estáticos o dinámicos. Un inventario dinámico utiliza scripts para obtener una lista de hosts desde fuentes externas, como AWS, Google Cloud, etc. Estos scripts generan la lista de hosts y sus variables en tiempo real.
- Grupos anidados: Puedes crear grupos de grupos. Por ejemplo, podrías definir un grupo datacenter que contenga `webservers` y `dbservers`.

```ini
[datacenter:children]
webservers
dbservers
```

Una vez definido el archivo de inventario, puedes usarlo con comandos de Ansible como ansible o ansible-playbook. Ejemplo:

```bash
ansible-playbook -i inventory.ini playbook-name.yml
```
