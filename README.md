<div align="center">

# Guardian Skills

<img src="https://img.shields.io/badge/GuardianContext-0.1.20-238636?style=for-the-badge" alt="GuardianContext 0.1.20">
<img src="https://img.shields.io/badge/skills-3-58a6ff?style=for-the-badge" alt="3 skills">
<img src="https://img.shields.io/badge/license-IronStack-0d1117?style=for-the-badge" alt="IronStack">

<span style="color:#c9d1d9">Skills that help coding agents use <span style="color:#58a6ff">GuardianContext</span> well.</span>

[English](#english) · [Español](#español) · [Português (Brasil)](#português-brasil)

</div>

---

<a name="english"></a>

## English

<span style="color:#58a6ff">### Three small skills, one job: keep your agent's work continuous.</span>

A context window ends. A session restarts. A handoff changes the actor. These skills give your agent a concrete procedure for each of those moments — and point to GuardianContext when the work needs versioned decisions, file-scope claims, and a record you can inspect.

They are **text only**. Installing them does not install the GuardianContext client or its backend. That is intentional: you can read the procedure first, then decide whether to install the tool.

| Skill | What it helps the agent do | GuardianContext commands it uses |
|---|---|---|
| `guardian-context` | Save a lightweight checkpoint before compaction or handoff, without storing a raw conversation transcript | `session-context checkpoint`, `conversation import/prepare/summarize/show` (explicit export only) |
| `guardian-resume` | Read the checkpoint and router back, then explicitly assume or renew a claim — or transfer it when the actor changed | `session-context status`, `claims resume`, `claims assume`, `claim-transfer --request/--confirm` |
| `guardian-claim-recovery` | Recover, transfer, or administratively abandon a claim, and choose a team policy when coordination is unavailable | `claims resume`, `claims assume`, `claim-transfer`, `claims abandon` |

<span style="color:#238636">### Step 1 — Install the skills</span>

Requires Node.js. Telemetry off by default in the example:

```sh
DISABLE_TELEMETRY=1 npx skills add RogerioTrani/guardian-skills@v0.1.20 --copy -a <your-agent>
```

Pin the version with `@v0.1.20`. Choose your agents with `-a`. To check what would be installed:

```sh
DISABLE_TELEMETRY=1 npx skills add RogerioTrani/guardian-skills --list
```

Update and remove:

```sh
npx skills update -g
npx skills remove guardian-context guardian-resume guardian-claim-recovery -g
```

<span style="color:#238636">### Step 2 — Install GuardianContext</span>

The skills are procedures; GuardianContext is the tool that stores the checkpoint, the claims, and the handoff. Install it in one repository and try it before anything else:

```sh
uv tool install guardiancontext
guardian --version
cd <your-repository>
guardian init
guardian doctor
guardian skills status   # prints the exact commands to register the skills globally
```

See [the user manual](https://ironstack.dev.br/guardiancontext/manual/) for Desktop Manager, native hooks, and optional workflows. `guardian doctor` reports local setup and server contact separately — a ready configuration is not proof that an agent read or followed a rule.

<span style="color:#58a6ff">### What these skills do not promise</span>

- They do not install the client or backend. `npx skills` copies text only.
- They do not store raw conversation transcripts by default. Export synthesis happens only when you select an export file.
- They do not guarantee that an agent followed an instruction, that conflicts cannot happen, or that your bill or development time will drop. Configuration and recorded activity can be inspected; compliance cannot be assumed.
- The actor label is declared, not authenticated. A claim is a time-limited reservation of file scope, not proof of identity or complete enforcement.

GuardianContext is an IronStack product under the proprietary license included in the package. Service conditions: [Terms of Use](https://api.ironstack.dev.br/termos). [Product website](https://ironstack.dev.br/guardiancontext/) · [PyPI](https://pypi.org/project/guardiancontext/) · [Email support](mailto:suporte@ironstack.dev.br)

---

<a name="español"></a>

## Español

<span style="color:#58a6ff">### Tres skills pequeñas, un trabajo: mantener continuo el trabajo de tu agente.</span>

La ventana de contexto termina. Una sesión se reinicia. Un traspaso cambia el actor. Estas skills dan a tu agente un procedimiento concreto para cada uno de esos momentos y apuntan a GuardianContext cuando el trabajo necesita decisiones versionadas, reservas de archivos y un registro que puedas inspeccionar.

Son **solo texto**. Instalarlas no instala el cliente ni el backend de GuardianContext. Eso es deliberado: puedes leer el procedimiento primero y decidir después si instalas la herramienta.

| Skill | Qué ayuda a hacer al agente | Comandos de GuardianContext que usa |
|---|---|---|
| `guardian-context` | Guardar un checkpoint ligero antes de compactar o traspasar, sin almacenar la transcripción cruda de la conversación | `session-context checkpoint`, `conversation import/prepare/summarize/show` (solo exportación explícita) |
| `guardian-resume` | Reler el checkpoint y el router, y asumir o renovar explícitamente una reserva — o transferirla cuando cambió el actor | `session-context status`, `claims resume`, `claims assume`, `claim-transfer --request/--confirm` |
| `guardian-claim-recovery` | Recuperar, transferir o abandonar administrativamente una reserva, y elegir la política de equipo cuando la coordinación no está disponible | `claims resume`, `claims assume`, `claim-transfer`, `claims abandon` |

<span style="color:#238636">### Paso 1 — Instalar las skills</span>

Requiere Node.js. Telemetría desactivada en el ejemplo:

```sh
DISABLE_TELEMETRY=1 npx skills add RogerioTrani/guardian-skills@v0.1.20 --copy -a <tu-agente>
```

Fija la versión con `@v0.1.20`. Elige tus agentes con `-a`. Para ver qué se instalaría:

```sh
DISABLE_TELEMETRY=1 npx skills add RogerioTrani/guardian-skills --list
```

Actualizar y eliminar:

```sh
npx skills update -g
npx skills remove guardian-context guardian-resume guardian-claim-recovery -g
```

<span style="color:#238636">### Paso 2 — Instalar GuardianContext</span>

Las skills son procedimientos; GuardianContext es la herramienta que guarda el checkpoint, las reservas y el traspaso. Instálalo en un repositorio y pruébalo antes que nada:

```sh
uv tool install guardiancontext
guardian --version
cd <tu-repositorio>
guardian init
guardian doctor
guardian skills status   # muestra los comandos exactos para registrar las skills globalmente
```

Consulta el [manual de uso](https://ironstack.dev.br/guardiancontext/manual/) para Desktop Manager, hooks nativos y flujos opcionales. `guardian doctor` distingue la configuración local del contacto con el servidor: una configuración lista no demuestra que un agente haya leído o seguido una regla.

<span style="color:#58a6ff">### Lo que estas skills no prometen</span>

- No instalan el cliente ni el backend. `npx skills` copia solo texto.
- No guardan transcripciones crudas por defecto. La síntesis de exportaciones ocurre solo cuando tú seleccionas un archivo.
- No garantizan que un agente siga una instrucción, que no puedan ocurrir conflictos, ni que tu factura o tu tiempo de desarrollo bajarán. La configuración y la actividad registrada se pueden inspeccionar; el cumplimiento no se puede suponer.
- La etiqueta del actor se declara, no se autentica. Una reserva es una limitación de archivos con duración determinada, no prueba de identidad ni cumplimiento completo.

GuardianContext es un producto de IronStack bajo la licencia propietaria incluida en el paquete. Condiciones del servicio: [Términos de Uso](https://api.ironstack.dev.br/termos). [Sitio del producto](https://ironstack.dev.br/guardiancontext/) · [PyPI](https://pypi.org/project/guardiancontext/) · [Soporte por correo](mailto:suporte@ironstack.dev.br)

---

<a name="português-brasil"></a>

## Português (Brasil)

<span style="color:#58a6ff">### Três skills pequenas, um trabalho: manter contínuo o trabalho do seu agente.</span>

A janela de contexto termina. Uma sessão reinicia. Uma transferência troca o ator. Estas skills dão ao seu agente um procedimento concreto para cada um desses momentos e apontam para o GuardianContext quando o trabalho precisa de decisões versionadas, reservas de arquivos e um registro que você possa inspecionar.

Elas são **somente texto**. Instalá-las não instala o cliente nem o backend do GuardianContext. Isso é intencional: você pode ler o procedimento primeiro e decidir depois se instala a ferramenta.

| Skill | O que ajuda o agente a fazer | Comandos do GuardianContext que usa |
|---|---|---|
| `guardian-context` | Salvar um checkpoint leve antes da compactação ou transferência, sem armazenar a transcrição bruta da conversa | `session-context checkpoint`, `conversation import/prepare/summarize/show` (somente exportação explícita) |
| `guardian-resume` | Reler o checkpoint e o router e assumir ou renovar explicitamente uma reserva — ou transferi-la quando o ator mudou | `session-context status`, `claims resume`, `claims assume`, `claim-transfer --request/--confirm` |
| `guardian-claim-recovery` | Recuperar, transferir ou abandonar administrativamente uma reserva, e escolher a política de equipe quando a coordenação não está disponível | `claims resume`, `claims assume`, `claim-transfer`, `claims abandon` |

<span style="color:#238636">### Passo 1 — Instalar as skills</span>

Requer Node.js. Telemetria desligada no exemplo:

```sh
DISABLE_TELEMETRY=1 npx skills add RogerioTrani/guardian-skills@v0.1.20 --copy -a <seu-agente>
```

Fixe a versão com `@v0.1.20`. Escolha seus agentes com `-a`. Para ver o que seria instalado:

```sh
DISABLE_TELEMETRY=1 npx skills add RogerioTrani/guardian-skills --list
```

Atualizar e remover:

```sh
npx skills update -g
npx skills remove guardian-context guardian-resume guardian-claim-recovery -g
```

<span style="color:#238636">### Passo 2 — Instalar o GuardianContext</span>

As skills são procedimentos; o GuardianContext é a ferramenta que guarda o checkpoint, as reservas e a transferência. Instale-o em um repositório e experimente antes de qualquer coisa:

```sh
uv tool install guardiancontext
guardian --version
cd <seu-repositorio>
guardian init
guardian doctor
guardian skills status   # mostra os comandos exatos para registrar as skills globalmente
```

Veja o [manual de uso](https://ironstack.dev.br/guardiancontext/manual/) para Desktop Manager, hooks nativos e fluxos opcionais. O `guardian doctor` mostra separadamente a configuração local e o contato com o servidor — uma configuração pronta não demonstra que um agente leu ou seguiu uma regra.

<span style="color:#58a6ff">### O que estas skills não prometem</span>

- Não instalam o cliente nem o backend. O `npx skills` copia somente texto.
- Não armazenam transcrições brutas por padrão. A síntese de exportação acontece somente quando você seleciona um arquivo.
- Não garantem que um agente seguiu uma instrução, que conflitos não possam acontecer, nem que sua fatura ou seu tempo de desenvolvimento vão cair. Configuração e atividade registrada podem ser inspecionadas; cumprimento não se presume.
- O rótulo do ator é declarado, não autenticado. Uma reserva é uma limitação de arquivos com duração determinada, não prova de identidade nem de aplicação completa.

O GuardianContext é um produto da IronStack sob a licença proprietária incluída no pacote. Condições do serviço: [Termos de Uso](https://api.ironstack.dev.br/termos). [Site do produto](https://ironstack.dev.br/guardiancontext/) · [PyPI](https://pypi.org/project/guardiancontext/) · [Suporte por e-mail](mailto:suporte@ironstack.dev.br)
