# docker-php-apache2-magento2

<p>Docker não é um sistema de virtualização tradicional. Enquanto em um ambiente de virtualização tradicional nós temos um S.O. completo e isolado, dentro do Docker nós temos recursos isolados que utilizando bibliotecas de kernel em comum (entre host e container), isso é possível pois o Docker utiliza como backend o nosso conhecido LXC.<p>

<p>Docker é uma plataforma Open Source escrito em Go, que é uma linguagem de programação de alto desempenho desenvolvida dentro do Google, que facilita a criação e administração de ambientes isolados.<p>

<p>Mas por que que o Docker é tão legal?<p>

<p>O Docker possibilita o empacotamento de uma aplicação ou ambiente inteiro dentro de um container, e a partir desse momento o ambiente inteiro torna-se portável para qualquer outro Host que contenha o Docker instalado.<p>

<p>Isso reduz drasticamente o tempo de deploy de alguma infraestrutura ou até mesmo aplicação, pois não há necessidade de ajustes de ambiente para o correto funcionamento do serviço, o ambiente é sempre o mesmo, configure-o uma vez e replique-o quantas vezes quiser.<p>

<p>Outra facilidade do Docker é poder criar suas imagens (containers prontos para deploy) a partir de arquivos de definição chamados Dockerfiles.<p>

<p>Não podemos nos esquecer também de que o Docker utiliza como backend default o LXC, com isso é possível definir limitações de recursos por container (memória, cpu, I/O, etc.)<p>

<p>Como o Docker faz isso?<p>

<p>Como ele trabalha utilizando cliente e servidor (toda a comunicação entre o Docker Daemon e Docker client é realizada através de API), basta apenas que você tenha instalado o serviço do Docker em um lugar, e aponte em seu Docker Client para esse servidor. A plataforma do Docker em si utilizada alguns conjuntos de recursos, seja para a criação ou administração dos containers, entre esses conjuntos podemos destacar a biblioteca libcontainer, que é responsável pela comunicação entre o Docker Daemon e o backend utilizado, é ela a responsável pela criação do container, e é através dela que podemos setar os limites de recursos por container.<p>

# Agora vamos praticar?

<p>Comece atulizando seu ubuntu<p>

<pre>#apt-get update</pre>

<pre>#apt-get upgrade</pre>

<p>Agora vamos instalar uma imagem extra virtual do ubuntu. <p>

<pre>#apt-get install -y \ linux-image-extra-$(uname -r) \ linux-image-extra-virtual</pre>

<pre>#apt-get install -y \ apt-transport-https \ ca-certificates \ curl \ software-properties-common</pre>

<p>Agora vamos instalar o docker.<p>

<pre>#apt install -y docker.io git</pre> 

<p>Vamos instalar o docker-compose.<p>

<pre>#curl -L "https://github.com/docker/compose/releases/download/1.11.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose</pre>

<p>Vamos dar permissão de execução para o docker-compose.<p>

<pre>#chmod +x /usr/local/bin/docker-compose</pre>

<p>Vamos clonar um diretorio.<p>

<pre>#git clone https://github.com/arvindr226/magento2-developer</pre>

<p>Vamos entrar no diretorio que acabamos de clonar.<p>

<pre>#cd magento2-developer</pre>

<p>Vamos clonar um container do github, para dentro de uma pasta html.<p>

<pre>#git clone https://github.com/magento/magento2.git  html</pre>

<p>Vamos dar start em nosso container recem clonado.<p>

<pre>#docker-compose up -d</pre>

<p>Ao dar start no container ele ira realizar o clone das imagens necessárias na primeira vez. Serão as imagens como arvindr226 / m2, phpmyadmin / phpmyadmin, mysql. Em seguida, ele criará recipientes conforme configurado no arquivo docker-compose.yml.<p>

<p>Pronto seu docker está quase pronto realize um teste em seu browser digitando:<p>

<p>localhost ou seu IP
Se o retorno for esse vá para o proximo passo:<p>

<pre>Autoload error</pre>

<p>Precisamos agora instalar os coponentes do magento2 com o docker-composer:<p>

<pre>#docker exec -it magento2-developer composer install</pre>

<p>Após a instalção se concluir teste novamente em seu browser:<p>

<pre>seu ip /setup</pre>

<p>Abrirá a pagina de instalação do magento antes de fazer qualquer coisa vamos dar permissão de execução para os novos componentes instalados:<p>

<pre>#chmod -R 777 html/app/ html/var/ html/pub</pre>

<p>Agora clique em AGREE AND SETUP MAGENTO. O magento fará a verificação do sistema possivelmente da erro de permissão em um dos arquivos do php, para ser a permissão vamos entrar no container instalado:<p>

<pre>#docker exec -ti magento2-developer bash</pre>

<p>Agora estamos dentro do container instalado, vamos dar permissão ao arquivo:<p>

<pre>#chmod -R 777 /var/www/html/generated</pre>

<p>Volte ao browser e atualize agora irá aprovar todos os 5 componentes, clique e será solicitada o nome do banco de dados e a senha:<p>

<pre>DATABASE SERVER HOST: magento2_bd
DATABASE SERVER USERNAME: magento2
DATABASE SERVER PASWWORD: gotchnies
DATABASENAME: magento2</pre>

<p>CLique em Next crie sua conta de administrador, e clique em next ai começará a instalar o magento.<p>

<p>Pronto só criar sua loja.<p>


