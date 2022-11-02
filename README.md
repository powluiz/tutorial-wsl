# Comandos base

## 🐧 WSL

- Obter atualizações dos pacotes instalados: `sudo apt update`
- Conferir diretório atual: `pwd`
- Visualizar diretórios ocultos: `ls -a`
- Ver o histórico de comandos: `history`
- Ir para diretório: `cd nome`
- Recuar diretório: `cd ..`
- Criar nova pasta: `mkdir nome-pasta`
- Remover pasta: `rm -rf nomepasta`
- Para criar um arquivo: `touch nome-arquivo`
- Abrir o arquivo com editor nativo (nano): `nano nome-arquivo`
- Visualizar o texto de um arquivo: `cat nome-arquivo`
- Guia sobre comandos: `man nome-comando`
- Para alternar entre o bash e cmd em um mesmo prompt:
  `wsl.exe` ou `cmd.exe`.

  OBS: Ao abrir o bash pelo prompt padrão do windows, ele vai estar no mesmo diretório C://...

<br>

# Para compartilhar chaves SSH entre Windows e WSL:

👉 Baseado no tutorial <a href="https://devblogs.microsoft.com/commandline/sharing-ssh-keys-between-windows-and-wsl-2/">deste link</a> .

## Copiar do Windows para WSL:

Adicione suas chaves SSH no windows dentro do endereço desejado (Normalmente `C:\Users\<username>\.ssh`). Para saber como gerar de forma local e adicionar sua chave SSH na lista de permissões, veja a documentação do servidor que utilizar (GitHub, GitLab, Bitbucket, etc).

Em seguida, abra um novo terminal no WSL (e que esteja no diretorio raíz, como: `Linux/sua-distro/home/seu-username`) e utilize o comando a seguir para copiar a sua pasta .ssh do windows para o WSL:

```
cp -r /mnt/c/Users/<username>/.ssh ~/.ssh
```

OBS: Isso significa que, ao gerar uma nova chave ssh no windows, o comando acima deve ser executado novamente. De forma alternativa, você pode manualmente copiar a pasta .ssh e colar no diretório dentro da sua pasta de usuário no WSL.

## Correção de Permissões

Para corrigir os erros de permissão de acesso, utilize o comando a seguir:

```
chmod 600 ~/.ssh/id_rsa
```

A partir de agora, para facilitar o controle das suas chaves SSH, vamos utilizar o <a href = "https://www.funtoo.org/Funtoo:Keychain">Keychain</a>. Para instalá-lo, execute:

```
sudo apt install keychain
```

Em seguida, vá até o arquivo `~/.bashrc` e adicione o seguinte código:

```

eval `keychain --eval --agents ssh id_rsa`

```

👉 Para realizar o passo acima, você pode utilizar o comando `nano ~/.bashrc` para abrir o editor de texto via terminal e adicionar o código no final do arquivo. Para salvar, pressione `ctrl + x`, depois `y` e, por fim, `enter` para confirmar a gravação.

## E para mais de uma chave?

Por vezes pode ser necessária a adição de mais de uma chave SSH (quando se deseja acessar múltiplas contas no GitHub ou mesmo, usar contas em diferentes servidores).

Para isso, na hora de gerar a chave, ao invés de utilizar o nome padrão `id_rsa`, utilize um nome diferente, como: `id_gitlab_rsa`, por exemplo. Depois é só seguir o mesmo passo a passo (copiar a pasta .ssh do windows para o wsl, corrigir as permissões e adicionar o código no `~/.bashrc`).

Nesse caso, o final do `.bashrc` ficaria assim:

```
eval `keychain --eval --agents ssh id_rsa id_gitlab_rsa id_outra_chave_rsa`
```

## Como saber se deu certo?

Toda vez que iniciar o WSL, o terminal deve exibir uma mensagem como a seguinte:

<img src = "./src/img/keychain.png"> </img>

Pronto! Agora você pode usar as chaves SSH no WSL sem mais dores de cabeça :) .

`Para mais informações sobre como utilizar um nome diferente de id_rsa ou como definir uma senha em sua criação, por exemplo, leia a documentação do GitHub, GitLab, ou outro servidor que utilizar!`
