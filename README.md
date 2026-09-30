# WMU downloads

Baixe [wmu-launcher.exe](https://github.com/worthdavi/wmu-downloads/releases/latest/download/wmu-launcher.exe)
e abra. O launcher instala, verifica e atualiza o client automaticamente.
Windows x64; o jogador não precisa de Git ou ferramentas de desenvolvimento.

## Organização

A branch contém documentação e uma cópia de referência do `feed.json`.
ZIPs e executáveis são anexos das [Releases](https://github.com/worthdavi/wmu-downloads/releases).
O launcher consulta o **feed anexado à última Release**, baixa um ZIP completo
e verifica SHA-256 antes de instalar; não baixa arquivo por arquivo pelo Git.
Um commit/push sozinho não publica uma atualização.

`client/windows-x64/` e `releases/` são pastas locais ignoradas pelo Git.
A primeira contém o runtime de trabalho; a segunda, os pacotes para upload.
Os arquivos locais foram preservados. O histórico antigo continua intacto;
para um clone novo sem os antigos binários, use `git clone --depth 1`.

O runtime inclui `wmu.exe`, DLLs necessárias, `client.toml`, `keys.toml`,
`assets/`, `data/`, `game/Data/`, `licenses/` e `release-manifest.json`.
São 3.592 arquivos na versão 0.3.0. Código-fonte, ferramentas, arquivos de
autoria, símbolos de depuração e executáveis legados não entram no ZIP.

O client é uma compilação Release x64 com subsistema gráfico Windows:
abre somente a janela do jogo, sem console de logs. O empacotador verifica
essa propriedade no próprio executável antes de aceitar uma nova versão.

## Conexão com o servidor

No launcher 0.2.0, clique em **Servidor** no rodapé:

- **Padrão do jogo:** conecta ao servidor Fly.io em `server.wmu.life`.
- **Localhost:** usa `127.0.0.1`, conexão `44405` e porta base do jogo `55901`.
- **Meu servidor:** informe IP/domínio, portas e, se necessário, a CA pública.

A escolha persiste em `%LOCALAPPDATA%/WMU/server-settings.json`, sobrevive a
atualizações/reparo e vale na próxima abertura do jogo. Os canais preservam
seus deslocamentos de porta. O servidor precisa estar acessível e anunciar
endereços corretos; o launcher não cria um servidor de jogo. A validação TLS
permanece ativa. O client 0.3.0 usa o protocolo WMU3 e requer o servidor Go;
o launcher existente baixa essa atualização automaticamente pela Release latest.

Login, personagens, movimentação, renderização de Lorencia, HUD, comércio,
grupo, baú, loja e persistência foram validados contra o servidor Go em ambiente
isolado. Permanecem ausentes 26 assets customizados MuAwaY da distribuição
anterior; os dois testes desses assets continuam pendentes. Os assets existentes
foram preservados integralmente.

## Publicar uma atualização do client

No workspace privado `wmu`, com Python 3.11+:

1. Exporte a nova compilação para `downloads/client/windows-x64`, preservando
   os assets necessários. Revise as configurações e teste o jogo.
2. Gere manifesto e ZIP com uma versão e tag novas:

```powershell
python client/scripts/releasefiles.py create downloads/client/windows-x64 --platform windows-x64 --version 0.3.0
python launcher/scripts/packageclient.py --version 0.3.0 --release-url https://github.com/worthdavi/wmu-downloads/releases/download/client-v0.3.0 --output downloads/releases/client-0.3.0
Copy-Item downloads/releases/client-0.3.0/feed.json downloads/feed.json
```

Use `--allow-local-server` no empacotador para distribuir intencionalmente
uma configuração local. Não regenere manifestos para encobrir corrupção.

3. Atualize as notas, faça commit/push **dos metadados** e compile o launcher:

```powershell
powershell -File launcher/scripts/build.ps1
```

4. Publique com o commit completo de `downloads` já enviado ao GitHub:

```powershell
python launcher/scripts/githubrelease.py --directory downloads/releases/client-0.3.0 --tag client-v0.3.0 --commit COMMIT_COMPLETO --notes downloads/release-notes.md --launcher launcher/dist/launcher/wmu-launcher.exe
```

O publicador usa seu login do Git, cria um rascunho, envia ZIP, checksum, feed
e launcher, verifica os uploads e só então torna a Release pública/latest.
Não substitua pacotes já publicados. Atualizações e reparos baixam o ZIP
completo; patches incrementais ficam para outra etapa.

O feed estável é:
`https://github.com/worthdavi/wmu-downloads/releases/latest/download/feed.json`.
Uma Release somente do launcher também deve incluir esse feed apontando ao
ZIP imutável do client existente. O launcher atualiza o client; uma nova
versão do próprio launcher deve ser baixada pelo link de download acima.

## Recuperar o runtime em outra máquina de desenvolvimento

Baixe o ZIP indicado no `feed.json` e use o SHA-256 desse mesmo feed:

```powershell
python client/scripts/releasefiles.py stage CAMINHO_DO_ZIP downloads/client/windows-x64 --platform windows-x64 --sha256 HASH_DO_FEED
```

O destino deve estar ausente. Esse comando verifica o pacote antes de
disponibilizar o runtime para uma próxima exportação.
