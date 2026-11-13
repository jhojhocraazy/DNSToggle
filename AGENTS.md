# AGENTS.md — Diretriz local de localização: DNSToggle

## 1. Domínio

Utilitário Android de baixo nível para alternar e automatizar configuração de rede — resolução de DNS,
interação com VPN, perfis automáticos e atalho no painel de configurações do sistema. Para funcionar,
depende de permissão privilegiada (`WRITE_SECURE_SETTINGS`, verificada no manifesto) concedida por ADB,
por serviço Shizuku ou por root (`su`).

Consequência para o texto: o app pede ao usuário que execute comandos no computador e conceda uma
permissão que o Android esconde. Instrução ambígua aqui não gera reclamação — gera app desinstalado
com review de uma estrela. Precisão vale mais que simpatia.

**Nome oficial do app: `DNS Toggle`, com espaço.** Não se traduz, não se flexiona e não vira
`DNSToggle` em texto visível. A chave `app_name` é `translatable="false"` na fonte EN e é espelhada com
o mesmo valor em pt-BR (não é uma string traduzível).

## 2. Onde o texto vive (verificado no clone)

| Caminho | Papel |
| --- | --- |
| `app/src/main/res/values/strings.xml` | fonte EN: 175 `<string>` (174 traduzíveis + `app_name`) e 1 `<plurals>` com 2 itens |
| `app/src/main/res/values-pt-rBR/strings.xml` | alvo pt-BR: espelha chaves, ordem e comentários da fonte EN |
| `fastlane/metadata/android/pt-BR/` | ficha da store: título ≤ 50, descrição curta ≤ 80, descrição longa ≤ 3700, changelog ≤ 500 |
| `build.gradle.kts` (`checkFastlaneMetadataLengths`) | gate real dos limites da Fastlane, executado por `preBuild` |
| `app/src/main/java/.../shizuku/ShizukuUserService.kt` | caminho privilegiado via Shizuku |
| `app/src/main/java/.../ui/dialog/PermissionDialogHelper.kt` | texto de concessão de permissão |
| `app/src/main/AndroidManifest.xml` | declaração de `WRITE_SECURE_SETTINGS` |

Os limites da ficha acima são os do build, não os do Play Console: `checkFastlaneMetadataLengths`
reprova título acima de 50, mas a Play Console ainda rejeita título acima de 30 — mantenha ≤ 30 na
prática. Descrição longa acima de 3700 falha no build mesmo que a Play aceite 4000.

Changelogs já publicados (`1.txt`–`36.txt`) são registro histórico e não se reescrevem para aplicar
vocabulário novo; a diretriz vale para textos novos ou revisados (a partir de `37.txt`).

## 3. Vocabulário nativo (prioridade sobre anglicismos)

O Android em pt-BR já tem rótulo próprio para cada uma dessas configurações. A evidência primária é a
tela de Configurações do aparelho; o app espelha esse vocabulário e não cria sinônimo.

| Conceito (EN) | Termo pt-BR obrigatório | Evidência / observação |
| --- | --- | --- |
| Private DNS | **DNS privado** | Configurações > Rede e internet > DNS privado |
| hostname | **nome do host** | Texto de ajuda "Nome do host do provedor" na mesma tela |
| Quick Settings | **Configurações rápidas** | Título do painel no Android pt-BR |
| Quick Settings tile | **bloco** | "Editar blocos" / "Layout dos blocos" |
| Wi-Fi profile | **perfil de Wi-Fi** | Vocabulário da ficha e da UI do app |
| SSID / network name | **nome da rede Wi-Fi** | Nunca "SSID" em texto visível ao usuário |
| Off (estado do app) | **Desligado** | Estado do interruptor; não confundir com o modo do Android |
| Disabled (modo do Android) | **Desativado** | Opção de DNS privado do próprio Android |
| Automatic (modo do Android) | **Automático** | Opção de DNS privado do próprio Android |
| VPN override | **DNS privado com VPN** | Descrever o comportamento; não traduzir "override" |
| override badge | **PRIORITÁRIO** | Selo do seletor de DNS (máx. 15 caracteres) |
| launcher | **tela inicial** | "App da tela inicial" nas configurações do Android |
| app drawer | **gaveta de apps** | Termo consagrado no Brasil; evitar "launcher" |
| toast | **aviso breve** / **notificação de status** | Evitar "toast" em texto visível |
| fallback | **exceção** / **alternativa** | Nunca "queda" ou "fallback" |

Regra de ouro: se o Android pt-BR já exibe um termo para a mesma configuração, ele vence qualquer
tradução criativa. Registrar aqui o termo novo antes de usá-lo pela primeira vez.

## 4. Protocolo de inferência semântica

1. **Ancorar no sistema.** A evidência primária é a tela de Configurações do próprio aparelho.
2. **Separar três registros:**
   - *rótulo de sistema* — usa o termo do Android, sempre;
   - *instrução de permissão* — imperativa, curta, com o comando exato em bloco de código;
   - *aviso de risco* — descreve o que muda no aparelho, sem alarmismo e sem promessa de reversão
     automática.
3. **Verificar o que o código realmente faz** antes de descrever. Se o app apenas escreve a
   configuração, o texto não pode dizer que "protege" ou "criptografa" o tráfego.
4. **Descrever comportamento, não metáfora.** "Override" vira a descrição do que acontece
   ("quando uma VPN estiver ativa, o DNS privado muda para…"), nunca "substituição"/"sobreposição".
5. **Tratar nomes próprios como nomes próprios**: `Shizuku`, `ADB`, `Fastlane`, `ChromeOS`,
   `Android Keystore` e `DNS Toggle` não se traduzem nem se flexionam.
6. **Registrar** o termo do sistema usado como fonte de cada rótulo de rede (§ 9).

## 5. Invariantes técnicas

- `%1$s`, `%s`, `%d`, `%1$d` preservados na mesma ordem da fonte; `\"`, `\n` e `&amp;` mantidos íntegros.
- Apóstrofo escapado quando existir (`\'`); `&` sempre como `&amp;`.
- Comandos ADB, nomes de pacote e chaves de `Settings.Secure` ficam literais, em bloco de código.
- Nome do app intacto: **`DNS Toggle`** (com espaço), `translatable="false"`, sem tradução.
- Arquivo pt-BR: UTF-8 sem BOM, quebras CRLF, indentação por tabulação, chaves na ordem exata da fonte EN
  e comentários explicativos traduzidos.
- Limites de texto na UI: rótulo de opção ≤ 30 caracteres; rótulo curto de atalho do launcher ≤ 10;
  rótulo longo de atalho ≤ 25; selo do seletor de DNS ≤ 15; ajuda ≤ 3 frases.
- Metadata da Fastlane respeita os limites do build (§ 2); changelog sem markdown que a Play rejeita.
- Toda alteração de string fecha com paridade de chaves: 175 `<string>` (174 traduzíveis + `app_name`)
  e 1 `<plurals>` com 2 itens.

## 6. Critérios de aceitação

- Instrução no imperativo direto ("Ative…", "Execute…"), sem "você deve".
- Nenhuma alegação de contornar bloqueio, operadora ou política de rede.
- Vocabulário de rede idêntico ao da tela de Configurações do Android em pt-BR.
- "Off" e "Disabled" nunca se confundem: `off_label` = "Desligado", `mode_disabled` = "Desativado",
  `off_strict_label` = "Desligado (Desativado)", `off_automatic_label` = "Desligado (Automático)".
- Texto que recebe nome de rede Wi-Fi (%s) não pode concordar em gênero com o nome: ancorar a
  concordância em um substantivo fixo (ex.: "DNS privado definido como %2$s").

## 7. Proibições

- Traduzir literalmente `tile`, `override`, `fallback` ou `hostname` em texto visível
  ("azulejo", "sobreposição", "queda", "hostname"): use `bloco`, descreva o comportamento, use
  "exceção" e "nome do host".
- Manter anglicismo solto quando o Android pt-BR tem termo equivalente (SSID, launcher, toast, tile).
- Chamar a permissão privilegiada de "root" quando o caminho real é Shizuku/ADB. Root é um caminho
  suportado (`grant_via_root`), então pode ser citado como opção — nunca como se fosse o único.
- Prometer segurança ou anonimato que o código não implementa.
- Editar string de tema (`values-night`) — não há texto lá.
- Traduzir nome de ferramenta (`Shizuku`, `ADB`, `Fastlane`, `ChromeOS`, `Android Keystore`).

## 8. Verificação

```powershell
# 1. Sintaxe XML do alvo
[xml](Get-Content -Raw 'app/src/main/res/values-pt-rBR/strings.xml') | Out-Null

# 2. Limites da ficha da store (mesmo gate do preBuild)
./gradlew checkFastlaneMetadataLengths

# 3. Build completo, quando o SDK estiver disponível
./gradlew :app:assembleDebug
```

Checklist: paridade de chaves e ordem, placeholders e escapes, limite de atalho (10/25) e selo (15),
vocabulário do sistema, comandos intactos, UTF-8 sem BOM + CRLF, nenhuma promessa indevida.

## 9. Handoff

| chave | EN | pt-BR escolhido | termo do Android equivalente | evidência (tela/doc) | risco se ambíguo |
| --- | --- | --- | --- | --- | --- |
| `private_dns` | Private DNS | DNS privado | DNS privado | Configurações > Rede e internet > DNS privado | "DNS Particular" não existe no sistema |
| `add_hostname` / `edit_hostname` | Add/Edit Hostname | Adicionar/Editar nome do host | Nome do host | Ajuda "Nome do host do provedor" | "hostname" cru quebra o padrão do sistema |
| `show_usb_debugging_tile` | Show USB Debugging tile | Mostrar bloco de Depuração USB | Bloco | "Editar blocos" no painel | "tile"/"ladrilho" não é usado no Android pt-BR |
| `off_label` | Off | Desligado | — | Estado do interruptor | Confundir com "Desativado" (modo do Android) |
| `mode_disabled` | Disabled | Desativado | Desativado | Opção de DNS privado do Android | "Desligado" apaga a distinção de modo |
| `off_strict_label` | Off (Disabled) | Desligado (Desativado) | Desativado | Opção homônima no aparelho | Perder a distinção entre estado e modo |
| `off_automatic_label` | Off (Automatic) | Desligado (Automático) | Automático | Opção homônima no aparelho | Idem |
| `status_override_ssid_profile` | SSID Override Active: %1$s is set to %2$s | Perfil de Wi-Fi ativo (%1$s): DNS privado definido como %2$s | Perfil de Wi-Fi / nome da rede | Cartão de status da tela principal | Concordar "definido" com o nome da rede quebra gênero |
| `badge_wifi_override` | OVERRIDE | PRIORITÁRIO | — | Selo em `item_dns_selection.xml` (≤ 15) | "Substituição" é tradução literal de override |
| `vpn_override_title` | VPN Override | DNS privado com VPN | — | Seção de VPN da tela principal | "Override"/"Substituição" não é vocabulário do sistema |
| `shortcut_toggle_short` / `shortcut_select_short` | Toggle DNS / Select DNS | Alternar / Escolher | — | `res/xml/shortcuts.xml` (≤ 10 caracteres) | Rótulo maior que 10 é cortado pelo launcher |
| `ssid_hint` | SSID | Nome da rede | Nome da rede | Diálogo de Wi-Fi do Android | "SSID" é anglicismo técnico para o usuário |
| `permission_message` | To use this toggle, you must grant settings permissions through root, Shizuku, or run this ADB command… | Para usar esta opção, conceda a permissão para alterar as configurações do sistema via root, Shizuku, ou execute este comando ADB… | — | Diálogo de permissão + `PermissionDialogHelper.kt` | Instrução vaga gera permissão não concedida |

Toda string de permissão entra no handoff com o comando exato que ela acompanha:
`adb shell pm grant com.ericlowry.dnstoggle android.permission.WRITE_SECURE_SETTINGS`.
