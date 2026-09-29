WMU Client 0.2.0 para Windows x64.

- Conexão com o servidor Go pelo novo protocolo WMU3 sobre TLS 1.3.
- Servidor padrão em server.wmu.life, com certificado público incluído.
- Atualização de mundo, personagens, inventário, loja, baú, grupos e comércio.
- Client Release x64, sem console de debug; assets existentes preservados.

Abra o launcher para baixar o client 0.2.0 automaticamente ou clique em
Procurar atualizações. Não é necessário baixar o launcher novamente.
O updater verifica SHA-256 e instala o ZIP completo da nova versão.

Use Servidor → Padrão do jogo para acessar o servidor público. A escolha
anterior de Localhost ou Meu servidor permanece salva; esses destinos também
precisam executar o servidor Go compatível com WMU3.

Login, personagens, movimentação, renderização, HUD, comércio, grupo, baú,
loja e persistência foram validados em integração isolada com o servidor Go.
Os 26 assets customizados MuAwaY que já faltavam na distribuição anterior
continuam pendentes; os demais assets foram mantidos sem alterações.
