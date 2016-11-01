Título: [Open edX Brasil - Virtual Meetup 1](https://www.youtube.com/watch?v=YpbDTPAbPNg)

Data de Realização: 29/10/201



Resumo Hangout

É recomendável antes de ler este resumo dar uma olhada na doc oficial de instalação

e no post publicado pelo nosso amigo Paulo para entender alguns

passos:



https://pauloromano.wordpress.com/2016/10/28/iniciando-no-mundo-do-open-edx/





Neste hangout basicamente foi explicado o que é necessário para subir um ambiente 

de desenvolvimento.



Observação: Muitos passos abaixo já foram descritos muito bem no link acima pelo Paulo.



Após configurado o nfs-kernel-server, é necessário baixar o script de configuração install_stack.sh através

do comando abaixo:



curl -OL https://raw.githubusercontent.com/edx/configuration/open-release/eucalyptus.2/util/install/install_stack.sh







Importante: O scriptinstall_stack.sh irá baixar a máquina com a versão de desenvolvimento ou produção do edX.

Também prepara o host para digitar e entrar no site da plataforma (porém

não está dando certo).



Finalmente, podemos executar o script conforme abaixo:



bash install_stack.sh devstack open-release/eucalyptus.2



Agora, podemos subir a máquina de desenvolvimento pelo vagrant através

do comando abaixo:



vagrant up 

(Aproveitando, para quem não conhece o vagrant, basicamente é:

vagrant up  - Inicia a máquina; 

vagrant ssh - Conectar na máquina via SSH

vagrant halt - Desligar a máquina )







--> Estrutura de Diretórios do edX: 



P/ acessar a máquina:

vagrant ssh





ESTRUTURA DE DIRETÓRIOS:

A aplicação fica dentro da pasta /edx/

cd /edx/



Dentro desta passa contém arquivos de configuração, ambientes virtuais, 

código fonte.



Listando dentro da pasta edx, verificamos que existem as seguintes pastas:

ls 

app bin etc var



-->Na pasta /etc estão os arquivos de configuração de toda a plataforma.



Listando dentro da pasta /etc, verificamos que existem as seguintes pastas/arquivos:



ecommerce   ecomworker   edxapp   playbooks   programs.yml 

ecomworker.yml   elasticsearch   programs



Na pasta /app/edxapp está o código fonte da plataforma em si (LMS / CMS)



Observação: 

LMS (Learning Management System): Gestão de aprendizado- Visão do aluno. Quando instrutor, tem uma visão

diferenciada.   



CMS (Content Management System): Gestão de conteúdo. Produção de conteúdo.

Também conhecida como Studio.



(No vídeo no youtube, foi mostrado exemplos concretos do openEDX e de ambos

os conceitos acima, vale a pena conferir. A plataforma possui possibilidades

incríveis tais como: possibilidade de criação de perguntas pelo professor, problemas randômicos por aluno e o funcionamento

de respostas discursivas).



Qualquer sistema web precisa de várias configurações como secret key, 

conexão com banco de dados dentre outras.



Se conectar com o usuário edxapp, essas configurações de ambiente 

já serão feitas (facilidade provida pela máquina virtual).



paver possui vários comandos.



app: 



/edxapp: Código fonte da plataforma em si; LNS e CMS;

Abaixo segue uma breve descrição do que é cada um:



LMS: Gestão de aprendizagem - Visão do aluno. Quando instrutor, tem uma visão

diferenciada. 



CMS: Gestão de conteúdo. Produção de conteúdo.



-->Variáveis de Ambiente



Variáveis de ambiente devem conter a versão correta do Python a ser executada



Porém através do usuário edxapp, podemos ter todas

as variáveis de ambiente já configuradas:



sudo su edxapp [Já configura todas as variáveis de ambiente.]



Informações sobre o comando paver:

paver --h [Lista com vários comandos de manutenção do sistema e testes.]



Alguns parâmetros úteis do paver:



paver lms --fast (Inicializa a parte LMS do sistema)

paver cms --fast (Inicializa a parte CMS do sistema)

paver run_all_services --fast : Roda LMS e CMS ao mesmo tempo. Com isso, 

pode-se simular um ambiente de produção.



Observação: Porta do CMS na versão de dev é a 8001.



IMPORTANTE: Sem parâmetro --fast demora muito. Para testar coisas, utilizar o parâmetro --fast 

(dessa maneira, não instala todas as dependência do Python).





Feito tudo isso, teoricamente o ambiente já estará funcionando para testes. Registre um usuário

para fins de teste. No caso, criamos o usuário 'paulo'.



Executar o manage.py (precisa estar na pasta /edx-platform), passando os seguintes

parâmetros:

- lms ou cms;

- shell

- settings=aws

 

./manage.py lms shell --settings=aws

Conecta no shell





Para se tornar super user:



>>> from django.contrib.auth.models import User

>>> User.objects.all()

>>> me = User.objects.get(username='paulo')

>>> me.is_superuser = True

>>> me.is_staff = True

>>> me.is_active = True

>>> me.save()



Eis passo a passo o que foi feito acima:



1 - Importado o model user;

2- Mostra os usuários cadastrados de sistema;

3 - Obtém (atribuindo a uma variável me) o usuário pelo seu nome(username)(ATENÇÃO: o nome de usuário foi cadastrado no momento

que foi registrado na aplicação. Basta substituir 'paulo' pelo nome do seu usuário

cadastrado);

4 - Configurando usuário como superuser;

5 - Configurando usuário como staff;

6 - Configurando usuário como ativo;

7 - Salva as alterações;



Este usuário como é staff, terá senha padrão edx

Agora, acesse:

127.0.0.1/admin



E acesse com as seguintes credenciais:



User: staff

Password: edx



Porém entra só como usuário, e não tem permissão para editar.
