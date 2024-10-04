# CheatSheetsHackathonCOPA-2024

## Introducción

## Linux

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
