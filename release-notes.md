WMU Client 0.1.1 para Windows x64, com Launcher 0.2.0.

- Client recompilado em Release x64 como aplicação gráfica do Windows.
- Abrir o jogo, pelo launcher ou diretamente, não abre mais um console de logs.
- O empacotador agora rejeita executáveis de console antes da publicação.
- Assets, configurações de servidor e controles do launcher preservados.

Abra o launcher para atualizar automaticamente para o client 0.1.1 ou clique
em Procurar atualizações. O launcher 0.2.0 continua compatível; não precisa
ser baixado novamente. O updater baixa o ZIP completo da nova versão.

Verificados: cabeçalho PE de aplicação gráfica, janela SDL/D3D12 real sem
console associado e conexão ao endereço selecionado no launcher.

O jogo vem configurado para localhost. Para acessar outro servidor, use
Servidor → Meu servidor. O servidor precisa estar disponível e com TLS
configurado; login e gameplay online ainda não foram qualificados.
