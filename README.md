# GitHub com chave SSH

Caso não consiga se conectar via ssh por problemas de rede
utilize o ssh over https
[Using SSH over the HTTPS  port](https://docs.github.com/en/authentication/troubleshooting-ssh/using-ssh-over-the-https-port)

## Qual a diferença entre SSH e HTTP no GitHub:

A diferença entre o protocolo **HTTP** e o **SSH** é que o **HTTP** é um protocolo de transferência de hipertexto utilizado para acessar a internet e transferir arquivos. Já o **SSH** é um protocolo de rede seguro utilizado para acessar e gerenciar servidores remotos.

No Git, o protocolo **HTTP** é utilizado para clonar repositórios públicos e privados, mas é necessário autenticar-se a cada operação. Já o protocolo **SSH** é utilizado para clonar repositórios privados e públicos, e não é necessário autenticar-se a cada operação, pois a autenticação é feita por meio de chaves criptográficas.

O protocolo **SSH** é mais seguro do que o **HTTP**, pois utiliza criptografia para proteger a conexão entre o cliente e o servidor. Além disso, o protocolo **SSH** é mais rápido do que o **HTTP**, pois não precisa autenticar-se a cada operação.

Portanto, recomenda-se utilizar o protocolo **SSH** para clonar repositórios privados e públicos, pois é mais seguro e mais rápido do que o protocolo **HTTP**.

*Fonte: [Diferença entre SSH e HTTP](https://cursos.alura.com.br/forum/topico-diferenca-entre-ssh-e-http-289791).*

## Como criar uma chave SSH
Tanto no **Linux** quanto no **Windows** essa primeira etapa será a mesma.

1. Abra o terminal.
2. Cole o texto abaixo, substituindo o email usado no exemplo pelo seu email do GitHub.

Esse comando vai criar uma chave **SSH**, usando o email como *etiqueta*.

```
ssh-keygen -t ed25519 -C "your_email@example.com"
```

```
# isto deverá aparecer como resposta no terminal.
> Generating public/private ALGORITHM key pair.
```

Quando for solicitado ("Enter a file in which to save the key"), traduzindo ***"Inserir um nome de arquivo no qual salvar a chave"***, você pode pressionar **Enter** para aceitar o nome padrão do arquivo.

Se você já tiver uma chave **SSH** criada anteriormente antes desse tutorial, leia a [ETAPA 3](), essa etapa é importante pois se uma chave já existe, você não quer sobreescrevê-la. Caso contrário pule para [ETAPA 4](). 
```
> Enter a file in which to save the key (/home/YOU/.ssh/id_ALGORITHM):[Press enter]
```
Aperte **Enter**.

3. *Observe que se você criou chaves SSH anteriormente, o `ssh-keygen` pode pedir para você reescrever outra chave, nesse caso recomendamos criar uma chave **SSH** com nome personalizado. Para fazer isso, digite o local padrão do arquivo e substitua ***id_ALGORITHM*** pelo nome da sua chave personalizada. Ex: ***"id_minhaChaveSSH"***.*

4. O terminal irá pedir para que entre com uma chave de segurança. Digite uma senha, que será usada para desbloquear a sua chave **SSH** e aperte  **ENTER**, depois confirme a senha e aperte **ENTER** novamente.

*Nota*. Quando um conexão estiver sendo feita via SSH, seja pelo GitHub ou outro serviço você usará essa senha para afirmar que é você mesmo realizando a operação :D.
Nessa etapa você pode só apertar **Enter** sem digitar uma senha, o que é extremamente desaconselhável e estraga o propósito da segurança, mas ainda assim é válido.

```
> Enter passphrase (empty for no passphrase): [Type a passphrase]
> Enter same passphrase again
```

Feito isso a chave **SSH** está criada no computador, a próxima parte é adicioná-la ao agente que controla as chaves **SSH**, e o processo é tão simples quanto o anterior.

## Como adicionar uma chave SSH ao ssh-agent.
O seu computador precisa estar ciente, de quais chaves **SSH** estão registradas para uso no seu computador. As etapas abaixo descrevem como adicionar as chaves ao agente, consequentemente tornando elas ativas. 

Há formas de realizar o procedimento tanto em [**Linux**](#linux) quanto em [**Windows**](#windows).

### Linux

Antes de adicionar uma nova chave **SSH** ao `ssh-agent` para gerenciar suas chaves você deve verificar se ele está funcionando normalmente.

1. Inicie o `ssh-agent` no terminal, com o comando.

```
eval "$(ssh-agent -s)"
```
Isso irá aparecer no terminal, o número significa o numero do processo que executa o `ssh-agent`, siginifica que ele está rodando normalmente... Esse número não necessariamente será igual ao descrito.
```
> Agent pid 59566
```

2. Adicione sua chave privada **SSH** ao `ssh-agent`.

*Se você criou sua chave com um nome diferente, ou se você está adicionando uma chave existente que tem um nome diferente, substitua ***id_ed25519*** no comando pelo nome do seu arquivo de chave privada. No caso sendo a primeira vez, não há necessidade de mudar o nome do arquivo. só utilizar o mesmo nome usado no exemplo abaixo.*

Digite no terminal do **Linux** o seguinte código e aperte **ENTER**.
```
ssh-add ~/.ssh/id_ed25519
```
Pronto agora é só adicionar a chave ao GitHub, Siga essas instruções aqui [Adicionar chave ao ***GitHub***](#como-adicionar-a-chave-ssh-ao-github) .
### Windows

1. Entre em uma nova janela do ***PowerShell*** com privilégios de administrador elevados. Para fazer isso basta pesquisar no menu do **Windows** por ***PowerShell***, clicar com o botão direito sobre o programa e logo em seguida, clicar em executar como administrador.

Digite o código abaixo e aperte **Enter** .
```
Get-Service -Name ssh-agent | Set-Service -StartupType Manual Start-Service ssh-agent
```
2. Agora feche o terminal, e inicie um novo, sem permissões de administrador, simplesmente clicando no programa para executá-lo. E então, digite o código abaixo e aperte **Enter**.

Atenção, não esqueça de substituir *\<YOU\>* pelo nome do seu usuário no **Windows** .
```
ssh-add c:/Users/<YOU>/.ssh/id_ed25519
```
Exemplo: `ssh-add c:/Users/LuisFelipe/.ssh/id_ed25519`, no meu caso.

Pronto agora é só adicionar a chave ao GitHub, Siga essas instruções aqui [Adicionar chave ao ***GitHub***](#como-adicionar-a-chave-ssh-ao-github) .

## Como adicionar a chave SSH ao GitHub.
Para adicionar a chave **SSH** ao **GitHub, basta seguir um processo simples, que consitui somente de copiar o conteúdo da chave pública, e colar no painel de configurações no site do GitHub. Simples Assim.

As etapas abaixo te guiarão seguindo as intruções do próprio site do **GitHub**.

Aqui vou usar somente a forma de copiar o arquivo via `Linux` visto que aqui todos nós estamos aprendendo a usá-lo e temos também acesso ao terminal seja pelo **WSL**, **Maquina Virtual** ou até mesmo pelo **Git Bash** que executa comandos **shell** normalmente.

1. Abra o terminal...
2. Digite o comando abaixo. Se por algum motivo, você usou um nome diferente para sua chave **SSH** não esquece de alterar no código. Aperte **Enter**.
```
cat ~/.ssh/id_ed25519.pub
```
Como vcs devem imaginar 👀, o comando `cat` lê o arquivo que está no caminho `~/.ssh/id_ed25519.pub`, e ecoa no terminal.

3. Copie o código, selecionando com o mouse e apertando `ctrl + c`.
4. No canto superior direito de qualquer página no GitHub, clique na sua foto de perfil e depois em Configurações.
5. Na seção "Acesso" da barra lateral, clique em Chaves **SSH** e **GPG**.
6. Clique em Nova chave **SSH** ou Adicionar chave **SSH**.
7. No campo "Título", adicione um nome descritivo para a nova chave. Por exemplo, se estiver usando um laptop pessoal, você pode chamar essa chave de "Laptop pessoal".
8. Selecione o tipo de chave, autenticação ou assinatura.<br>
Aqui você pode simplesmente escolher ***autenticação***.
9. No campo "Chave", cole sua chave pública, que você copiou do terminal.
10. Clique em Adicionar chave **SSH**.
11. Se solicitado, confirme o acesso à sua conta no GitHub.

## 🎉🎉🎈🥳 Parabéns!!! 🥳🎈🎉🎉
### Você acabar de garantir que as suas interações com o GitHub através desse computador estarão autenticadas através do protocolo **SSH**.

[" Mas, o que é SSH? "](https://www.cloudflare.com/pt-br/learning/access-management/what-is-ssh/).

Referências: 

(DOCS, GITHUB, 2024. Disponível em: ["Adding a new SSH key to your GitHub account"](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account?platform=linux). Acessado em: 07 nov. 2024.)

(ALURA. Tópico Fórum, 2023. Disponível em: ["Diferença entre SSH e HTTPS"](https://cursos.alura.com.br/forum/topico-diferenca-entre-ssh-e-http-289791). Acessado em: 07 nov. 2024.)

(CLOUDFLARE. O que é SSH?, 2024. Disponível em: [O que é SSH? | Protocolo Secure Shell (SSH)](https://www.cloudflare.com/pt-br/learning/access-management/what-is-ssh/). Acesso em: 07 nov. 2024.)
