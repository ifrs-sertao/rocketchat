## RocketChat funcional por trás do Treafik 2.4 com ICP Edu Ativo
Este projeto pode implementa o RocketChat 4.0.0 +Mongo 4.2 em com Docker-Compose atrás do proxy reverso Traefik, já com as configurações 
do ICP Edu  do campus.

## Estrutura de arquivos e pastas

- docker-compose.yml é o arquivo principal para construção da pilha de serviços. Nesse caso constrói o ambiente RocketChat usando imagens oficias do Rocket e do Mongo.
- data/uploads é o volume (em _bind mount_) montado na própria pasta do projeto que armazena os arquivos de usuário
- mongodb-rocket é o volume nomeado ( montado em /var/lib/docker/volumes) que persiste o banco de dados.

> Importante! Em caso de backup e restauração, para garantir a persistência dos dados, é necessário cuidado especial com os volumes _uploads_ e mongodg-rocket.

## Variáveis Ambiente
O docker-compose chama um arquivo (env_file: env-rocket) que, nesse caso, precisa ser preenchido com as configurações básicas de acesso à plataforma. Por exemplo:
```bash
ADMIN_USERNAME=admin_rocket
ADMIN_PASS=senha_do_adm
ADMIN_EMAIL=mail@example.com

```

> Aparentemente o usuário **admin** é previamente bloqueado na plataforma, portanto é necessário definir um nome de usuário diferente.

### Pontos de Atenção 

- [ ] Ajustar as regras de Host para o domínio correto da tua aplicação ```traefik.http.routers.portainer.rule=Host(`portainer3.sertao.ifrs.edu.br```
- [ ] O domínio precisa ser resolvido por um servidor DNS válido
- [ ] Caso o serviço exponha mais de uma porta, é obrigatório setar a porta padrão nos Labels ```traefik.http.services.portainer.loadbalancer.server.port=9000"```
- [ ] Em nosso cenário, configurações de websecure e tls são obrigatórios
- [ ] Mapeie a rede traefik-net (obrigatório) e seus volumes 
