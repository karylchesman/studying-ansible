# Este é um projeto de estudo sobre Ansible

Eu usei docker para simular um 'ambiente' com 1 controller e 2 managed nodes.

#### Para acessar o container do controller:

```bash
docker exec -it ansible-controller bash
```

#### Gerar chave SSH:

Use o comando abaixo para gerar uma chave SSH sem passphrase:

````bash
ssh-keygen -t rsa -b 4096 -f id_rsa -N ""

# Componentes básicos do Ansible

- Control node, controla tudo, pode ser seu próprio pc, uma pipeline ci/cd e etc, desde que tenha o Ansible instalado - Linux (quase sempre, questão de compatibilidade)
- Managed nodes, máquinas que vão ser gerenciadas - Podem ser Linux ou Windows
- Inventário, arquivo que lista os managed nodes
- Módulos, blocos de código que executam ações nos managed nodes
- Tasks, ação que vai ser executadas nos managed nodes, podem ser compostas por módulos
- Plays, conjunto de tasks que são executadas em uma ordem específica
- Playbooks, arquivos em YAML que definem um conjunto de plays

[NOTE] Ansible também funciona em Ad-hoc mode, onde você pode executar comandos diretamente nos managed nodes sem a necessidade de criar playbooks.

Ex.: ansible localhost -m ping
Ex. 2: ansible all -i 'localhost:2222,' -m ping --private-key=./nodes/nginx/id_rsa -u ansible

## Inventário

No arquivo de inventário, você consegue agrupar os hosts e também as variáveis que serão usadas neles criando grupos de grupos.

## Usando módulos em Ad-hoc mode

Para rodar em ad-hoc com um modulo específico (usando inventário):

```bash
ansible all -i hosts -m apt -a "name=tree update_cache=true"
````

- "-m" é para especificar o módulo que será usado.
- "-a" é para passar argumentos para o módulo.

[NOTE] Alguns módulos podem precisar de privilégios elevados, para isso usa-se a flag "--become".
