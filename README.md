# milhas-config

Arquivos de configuração remota do app **Milhas Aéreas: Juntar e Emitir**, publicados no GitHub Pages.
O app baixa estes arquivos ao abrir (tempo limite de 5 s), guarda a última versão válida no celular
e, se nunca conseguiu baixar, usa a cópia embutida no app.

| Arquivo | Conteúdo |
|---|---|
| `config.json` | afiliado, milheiro de referência, promoções, datas fixas, milhas por trecho, avisos |
| `cartoes.json` | cartões que dão pontos (pontuação, programa, transferências, validade) |
| `programas.json` | programas de pontos e milhas (regras principais) |
| `politica_privacidade.html` | política de privacidade do app |

## Publicar pela primeira vez

1. No GitHub, abra o repositório `milhas-config` (público).
2. Envie os 4 arquivos desta pasta para a raiz do repositório (botão **Add file → Upload files**, ou `git push`).
3. Vá em **Settings → Pages**. Em **Source**, escolha **Deploy from a branch**, branch `main`, pasta `/ (root)`. Salve.
4. Em 1 ou 2 minutos os arquivos ficam em:
   - `https://SEU-USUARIO.github.io/milhas-config/config.json`
   - `https://SEU-USUARIO.github.io/milhas-config/cartoes.json`
   - `https://SEU-USUARIO.github.io/milhas-config/programas.json`
   - `https://SEU-USUARIO.github.io/milhas-config/politica_privacidade.html`
5. Abra cada endereço no navegador para conferir.
6. No app, troque `SEU-USUARIO` em `lib/core/config/app_constants.dart` (constantes `remoteConfigBaseUrl` e `privacyPolicyUrl`).

## Rotina de atualização (sem publicar nova versão do app)

1. Edite o arquivo no GitHub (ícone de lápis) ou localmente.
2. **Valide o JSON** antes de salvar (por exemplo, cole em https://jsonlint.com). Um arquivo malformado é ignorado pelo app, que continua com a versão anterior.
3. Mantenha o campo `"versao"` como número inteiro (≥ 1). Aumente o número a cada mudança importante e atualize `"atualizado_em"` (AAAA-MM-DD).
4. Faça o commit. O GitHub Pages publica em 1 ou 2 minutos (o cache do GitHub pode levar até 10 minutos).
5. Os usuários recebem a nova versão na próxima vez que abrirem o app com internet.

### Regras de conteúdo

- **Promoções** (`config.json → promocoes`): `{"titulo", "descricao", "inicio": "AAAA-MM-DD", "fim": "AAAA-MM-DD", "tipo": "transferencia|compra|clube|passagem", "link_oficial"}`. O app mostra só as que estão dentro do período. Use sempre o link do regulamento oficial.
- **Nunca** inclua preço de venda de milhas, sites de compra/venda de milhas ou links de afiliado para cartões.
- **Cartões e programas**: não invente números. Se não confirmar um valor no site oficial, deixe `null` (o app mostra "Consulte o site oficial"). Preencha `fonte_url` com a página oficial usada e `conferido_em` com a data da conferência. Troque `verificado` para `true` só depois de revisar.
- A aba Cartões fala **só de pontos e milhas**: não adicione renda mínima, aprovação, salas VIP ou benefícios premium.
- `anuidade_ref` é só um número para o simulador (em R$ por ano), sem análise de perfil.
- `afiliado.proxy_url`: se preenchido, o app usa esse endereço como intermediário de preços (Worker) no lugar do que está no código. Útil para trocar o Worker sem nova versão.
- `avisos`: lista de textos curtos que aparecem no topo do Início (ex.: `["Instabilidade na busca de preços hoje."]`).
