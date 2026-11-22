# claire-agents

> Automated mirror generated from [fofwisdom/claire-bible](https://github.com/fofwisdom/claire-bible) (`agent_clients/`). Do not edit files in this repository manually; submit changes to `fofwisdom/claire-bible`.

[한국어 안내](#antigravity) · [English](#antigravity) · [Antigravity GUI 가이드 (KO)](./docs/antigravity.ko.md) · [Antigravity GUI Guide (EN)](./docs/antigravity.en.md) · [Claude 가이드 (KO)](./docs/claude.ko.md) · [Claude Guide (EN)](./docs/claude.en.md)

Official Claire Bible agent plugins for OAuth 2.1 MCP connection, isolated source extraction, and knowledge graph queries.

## Supported Agents

- **`claire-antigravity`** (`antigravity`, `v0.1.2`): Antigravity plugin bundle for OAuth 2.1 MCP connection, isolated source extraction, and knowledge graph queries.
- **`claire-claude`** (`claude`, `v0.1.2`): Claude plugin for OAuth 2.1 MCP connection, isolated source extraction, and knowledge graph queries against your Claire Bible server.

<a id="antigravity"></a>
## Antigravity (GUI 우선 안내 / GUI-First Setup)

터미널이나 JSON 편집 없이 Antigravity 앱 화면만으로 적용할 수 있습니다.

- **상세 단계별 가이드 (사진 대신 메뉴명·단축키 기준 친절 안내)**:
  - 한국어: [Antigravity GUI 설치 및 연결 가이드](./docs/antigravity.ko.md)
  - English: [Antigravity GUI Installation & Setup Guide](./docs/antigravity.en.md)
- **GitHub Releases**: [Download `claire-antigravity-v0.1.2.zip`](https://github.com/fofwisdom/claire-agents/releases/latest)

### 방법 1 (가장 쉬움): Claire 웹에서 복사해 Antigravity 채팅창에 붙여넣기

1. Claire Bible 웹 **계정 설정**(`/settings/account`)에서 추출 모드를 `client`로 바꿉니다.
2. **MCP 설정**(`/settings/mcp`)에서 **Antigravity 빠른 설치 문구**를 복사해 Antigravity `+ New Conversation`에 붙여넣습니다.
3. 채팅에 뜬 카드의 **Sign in** 또는 **Customizations -> Installed -> Plugins -> Claire Bible (`Setup required`) -> Go to Accounts**에서 로그인합니다.
4. 첫 실행 시 `Allow using this MCP tool?` (`claire-antigravity_claire/...`) 창이 뜨면 **`3 Yes, and always allow`** 선택 후 **`Submit`**을 누르면 반복 확인 없이 동작합니다.

### 방법 2 (마우스 클릭 직접 설치): `.zip` + `mcp_config.json` 내려받아 넣기

1. [GitHub Releases](https://github.com/fofwisdom/claire-agents/releases/latest)에서 `claire-antigravity-v0.1.2.zip`을 내려받아 압축을 풉니다.
2. Claire `/settings/mcp`에서 **서버 연결 파일 내려받기**(`Download server config`)로 `mcp_config.json`을 받아 `claire-antigravity` 폴더 안에 넣습니다.
3. OS 파일 탐색기에서 Antigravity 플러그인 폴더(`~/.gemini/config/plugins` 또는 `%USERPROFILE%\.gemini\config\plugins`)에 `claire-antigravity` 폴더를 옮깁니다.
4. **Customizations -> Installed -> Plugins -> Claire Bible (`Setup required`) -> Go to Accounts**에서 OAuth 2.1 로그인을 완료합니다.

<a id="claude"></a>
## Claude (GUI Marketplace Setup)

- **상세 가이드**: [한국어 안내](./docs/claude.ko.md) · [English Guide](./docs/claude.en.md)

1. Claude **Marketplace management**에 `fofwisdom/claire-agents` 를 입력해 동기화한 뒤 **`claire-claude`** 를 설치합니다.
2. 커넥터(MCP) 설정에서 내 Claire Bible 서버 `/mcp` URL(`https://<your-claire-server>/mcp`)을 연결하고 OAuth 2.1(`readonly`, `write`)로 로그인합니다.

<details>
<summary>CLI &amp; Advanced Verification (터미널 / SHA256SUMS 고급 사용자용)</summary>

- Claude Code CLI: `/plugin marketplace add fofwisdom/claire-agents` then install `claire-claude`.
- Antigravity CLI / checksum check: download `claire-antigravity-v0.1.2.tar.gz` and `SHA256SUMS` from [GitHub Releases](https://github.com/fofwisdom/claire-agents/releases/latest), run `sha256sum -c SHA256SUMS --ignore-missing` (macOS: `shasum -a 256 -c SHA256SUMS --ignore-missing`), and extract into `~/.gemini/config/plugins/`.

</details>

## Server Authentication Model

This repository distributes static client instructions, subagents, skills, and templates only. Zero server tokens or secrets are included. Every Claire Bible instance shares these bundles and authenticates dynamically over OAuth 2.1 (`PKCE S256` or `/device`).
