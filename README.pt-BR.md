🇺🇸 [English](README.md) | 🇧🇷 [Português](README.pt-BR.md)

# VPN-configuration-EC2

Configuração de uma VPN gratuita (Pritunl) em uma instância EC2 com Ubuntu.

## Passo 1: Instalação da VPN (Pritunl)

- Com a EC2 criada e usando uma das seguintes versões do Ubuntu: 20.04 ou 22.04, use o usuário root para executar os comandos da documentação do Pritunl.
- Acesse https://pritunl.com/, desça a página e encontre a versão do seu Ubuntu. Siga os comandos até o final e, depois, use o comando `systemctl status pritunl` para verificar se o serviço está rodando.

## Passo 2: Configuração da VPN

- No painel das EC2, vá para o security group da sua instância VPN e libere as portas 80 e 443 para acesso público (`0.0.0.0/0`).
- Copie o IPv4 público da sua instância e cole no navegador para acessá-la. No primeiro acesso, aparecerá um comando para você resgatar o login e a senha do admin da VPN.
- Copie o comando e execute-o dentro da sua EC2, pegue o login e a senha e informe-os na tela de login do Pritunl.
- Dentro do painel do Pritunl, vá em **Servers** > **Add Server** > coloque o nome do seu servidor, apague o que está em **DNS Server** > clique em **Advanced** e desmarque todas as caixas que estiverem marcadas. Clique em **Add**.
- Abrirá um painel mostrando algumas informações. Abaixo desse painel haverá uma rota `0.0.0.0/0` criada; clique em **Remove Route**.
- Ao lado do painel estará um campo chamado **Port** com uma numeração UDP. Copie essa numeração, vá para o security group da sua EC2 VPN, crie uma inbound rule do tipo Custom UDP, cole o número copiado e libere para `0.0.0.0/0` (acesso público).
- Volte para o painel do Pritunl, clique na aba **Users** e em **Add Organization**. Dê um nome para a organização e clique em **Add**.
- Volte para a aba **Servers** e, no painel do seu servidor, clique em **Attach Organization** e selecione a organização criada no passo anterior.
- Clique em **Add Route** e insira o bloco CIDR da sua VPC na AWS. Ele pode ser encontrado em: VPC > Your VPCs > selecione a VPC que está utilizando e veja o **IPv4 CIDR**. Copie o IP e volte ao painel do Pritunl para adicioná-lo em **Add Route**.
- Clique em **Start Server** e a VPN já estará rodando na sua rede.

## Passo 3: Criação de Usuário e Uso da VPN pelo Client

- No painel do Pritunl, vá em **Users** e clique em **Add User**.
- Dê um nome para o usuário, selecione a organização criada e defina um PIN (apenas numérico). Clique em **Add**.
- Selecione o usuário criado e clique na opção **Click to Download Profile**. Um arquivo `.tar` será baixado no seu computador.
- Baixe o client do Pritunl em https://client.pritunl.com/ e instale-o.
- Com o client aberto, clique em **Import** > **Import Profile** > **Browse** e selecione o arquivo `.tar` do usuário que você baixou.
- Clique em **Connect** e insira o PIN.
- Após isso, você estará conectado à VPN e poderá acessar os servidores da rede da VPC pelo IP privado.
