WMU CLIENT - WINDOWS X64

Extract the entire ZIP before opening wmu.exe. Windows 10/11 x64 and a D3D12
compatible GPU/driver are required by the current renderer. This is a test
package, without an Authenticode signature or an installer.

The original MU installation is not included. Set paths.game in client.toml to
an existing installation containing Data/Player/Player.bmd, using forward
slashes, for example: game = "C:/Games/MU". You can also launch with:
  wmu.exe --game-path "C:/Games/MU"

Server selection uses data/ui/servers.toml. This package ships the local test
server entry. A public server requires its actual address and public TLS CA.
Do not install server private keys in this package.

build-info.json says whether this executable contains the online protocol.
An offline-fixture package cannot log into a game server. Its headless CI
smoke test validates loading and startup, not rendering or online gameplay.

release-manifest.json records packaged file hashes. It detects corruption;
it is not a publisher signature or an automatic updater. Verify the archive's
SHA256 against the value supplied by a trusted distributor before use.
