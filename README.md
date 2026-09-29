# WMU downloads

Repositório público dos arquivos distribuíveis do WMU para Windows x64.
Os fontes, ferramentas de desenvolvimento e arquivos de autoria ficam no
repositório privado do client.

## Download

Baixe [wmu-launcher.exe](https://github.com/worthdavi/wmu-downloads/releases/latest/download/wmu-launcher.exe)
e abra. O launcher baixa e valida os arquivos do client automaticamente;
nas próximas aberturas, verifica se há uma versão nova. Os botões permitem
procurar atualizações, reparar os arquivos e iniciar o jogo.

## Estado atual

O diretório `client/windows-x64` contém o executável Release, DLLs necessárias,
interface, modelos próprios, configurações, fontes tipográficas e licenças.
O manifesto `release-manifest.json` registra tamanho e SHA-256 de cada arquivo.
Os bytes dos arquivos do runtime são preservados pelo `.gitattributes`.

**Client 0.1.0:** 3.592 arquivos de runtime, incluindo os recursos de
personagens, mapas, monstros, imagens e sons em `client/windows-x64/game/Data`.
O ZIP, seu SHA-256, `feed.json` e o launcher são anexos da Release `client-v0.1.0`.

A conexão de jogo desta primeira versão usa `127.0.0.1` (servidor local).
O download e a atualização funcionam pela internet; entrar no jogo depende
de um servidor em execução no endereço configurado. Nenhum endereço público
de servidor de jogo foi fornecido. A inicialização D3D12 e os testes do acervo
passaram; login e gameplay online não foram qualificados.

## Estrutura

```text
client/windows-x64/
  wmu.exe                 executável do client reescrito
  *.dll                   SDL3 e runtime Microsoft Visual C++
  assets/                 imagens e modelos próprios utilizados pelo client
  data/                   interface, tabelas, idioma, fontes e CA pública
  client.toml             configuração da janela e conexão
  keys.toml               teclas
  game/Data/              recursos de personagens, mapas, monstros e sons
  licenses/               avisos de dependências
  release-manifest.json   integridade dos arquivos
```

`main.exe`, DLLs do client legado, `src`, `source`, PDBs e arquivos Blender
não fazem parte da distribuição. `client/data` e `client/assets` do projeto
privado continuam sendo entradas de build; os arquivos aqui são a exportação
para distribuição. Editar uma exportação não altera os fontes privados.

## Preparar uma próxima atualização

1. Exportar a nova compilação do client, preservando a pasta de recursos
   `client/windows-x64/game/Data` e as configurações revisadas da distribuição.
2. Configurar o host público em `client/windows-x64/client.toml` e os hosts
   do diretório e de todos os canais em `client/windows-x64/data/ui/servers.toml`.
   Incluir a CA pública correta em `client/windows-x64/data/network/ca.pem`;
   a chave privada do servidor nunca pertence a este repositório.
3. Validar o client real com esses assets e com o servidor acessível.
4. No workspace de desenvolvimento `wmu`, com Python 3.11+ disponível,
   gerar o manifesto da configuração revisada e o pacote:

```powershell
python client/scripts/releasefiles.py create downloads/client/windows-x64 --platform windows-x64 --version 0.1.1
python launcher/scripts/packageclient.py --version 0.1.1 --release-url https://github.com/worthdavi/wmu-downloads/releases/download/client-v0.1.1 --output downloads/releases/client-0.1.1
```

Não regenere o manifesto para encobrir corrupção; esse comando é para uma
nova versão cujas alterações de arquivos foram revisadas. O empacotador
recusa runtime adulterado, assets ausentes e endereços locais. Para distribuição
explícita de testes com servidor local, como a 0.1.0, use `--allow-local-server`.

5. Fazer commit/push do runtime atualizado. Criar uma Release no GitHub com
   tag `client-v0.1.1`, anexar **todos os três** arquivos de `releases/client-0.1.1`
   (ZIP, SHA-256 e `feed.json`) e só então
   publicar como latest. O ZIP e o feed são anexos da Release, não um commit
   de arquivos ZIP na branch. O upload deve terminar antes da publicação.
6. Compilar o launcher no workspace privado:

```powershell
powershell -File launcher/scripts/build.ps1
```

Enviar `launcher/dist/launcher/wmu-launcher.exe` ao jogador. O endereço estável
de atualização configurado no launcher é:

```text
https://github.com/worthdavi/wmu-downloads/releases/latest/download/feed.json
```

Um commit/push neste repositório registra os arquivos, mas **não publica uma
atualização automaticamente**. O launcher consulta os anexos da última Release.
Para as próximas atualizações, exporte a nova compilação, revise as configurações
e repita com uma versão, tag e pasta de saída novas. Não substitua um ZIP de
uma versão já publicada. Nesta primeira etapa, a atualização baixa o ZIP completo.
