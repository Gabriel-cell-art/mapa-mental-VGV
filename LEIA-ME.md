# Mapa mental — PROVECTA VGV

Um único arquivo, `mapa-mental.html`. Abra com duplo clique no navegador (Chrome, Edge, Safari ou Firefox). Funciona offline: sem internet, as fontes Inter e IBM Plex Mono caem para as fontes do sistema.

## Como adicionar um mapa novo

1. Abra `mapa-mental.html` num editor de texto.
2. Procure o comentário `MAPAS`. Logo abaixo estão os blocos já existentes: `map-corretor` (o mapa padrão) e `map-exemplo`.
3. Cole um bloco novo ao lado deles:

```html
<script type="application/json" id="map-incorporadora">
{
  "id": "incorporadora",
  "title": "Tráfego pago para vender imóveis — Incorporadora",
  "root": {
    "id": "n1",
    "text": "Texto curto do nó",
    "note": "Explicação opcional.\n\nSuporta parágrafos, **negrito** e linhas iniciadas com '- ' como lista.",
    "tag": "OBRIGATÓRIO",
    "collapsed": false,
    "children": []
  }
}
</script>
```

4. Salve e recarregue a página. O mapa aparece no seletor da barra superior.

Regras:

- O `id` do bloco precisa começar com `map-`. O ideal é usar `map-<id do mapa>`.
- O `id` do mapa precisa ser único. Ele também é a chave das edições salvas no navegador.
- `note`, `tag` e `collapsed` são opcionais. Os `id` dos nós também: sem eles, o app gera um id novo a cada abertura e as edições salvas no navegador se perdem. Use ids fixos nos mapas reais.
- `"collapsed": true` define quais ramos abrem fechados. **Todo mapa abre sempre nesse estado**: o app não lembra os ramos que você abriu ou fechou na última vez.
- Dentro de `note`, quebra de linha se escreve `\n`, e um parágrafo novo é `\n\n`.
- JSON inválido não quebra a página: o bloco é ignorado e aparece um aviso na tela. Vírgula sobrando no final de uma lista é o erro mais comum.
- Link direto para um mapa: `mapa-mental.html#corretor`. Ao recarregar, o app continua no mapa aberto, porque o `#id` fica no endereço.

### Mapa padrão

Sem `#id` no endereço, abre o bloco que tem o atributo `data-default`. Hoje é o `corretor`:

```html
<script type="application/json" id="map-corretor" data-default>
```

Para trocar o padrão, mova o `data-default` para outro bloco. Deixe o atributo em um bloco só; sem nenhum, abre o primeiro bloco do arquivo.

Também dá para montar o mapa no próprio app (modo edição), exportar em **Exportar ▾ → Dados JSON** e colar o conteúdo num bloco novo.

### Cuidado: edição local x arquivo

As edições feitas no app ficam salvas **só neste navegador** (localStorage), separadas por `id` de mapa. Enquanto houver edição local, ela aparece **no lugar** do conteúdo do arquivo. Se você atualizar o bloco no HTML, o app avisa que o original mudou e continua mostrando a versão local até você clicar em **"Editado · Restaurar original"**. Para tornar uma edição permanente, exporte o JSON e cole no arquivo.

## Atalhos

Pressione `?` no app para ver esta lista.

### Navegação
| Atalho | Ação |
|---|---|
| Arrastar área vazia / scroll do trackpad | mover o mapa |
| `Ctrl/⌘` + roda, pinça, `+` / `−` | zoom (20% a 300%) |
| `F` | enquadrar tudo |
| `0` | zoom 100% centrado na raiz |
| Setas | mover a seleção (← pai, → filho, ↑↓ irmãos) |
| Clique num nó | selecionar e centralizar |
| `Espaço` | abrir/fechar o ramo selecionado |
| Clique no círculo à direita do nó | abrir/fechar o ramo |
| `Shift` + `1`…`4` | expandir até o nível N |

### Notas, foco e busca
| Atalho | Ação |
|---|---|
| `N` | abrir/fechar o painel de notas |
| `Alt` + clique | modo foco (escurece o resto) |
| `Ctrl/⌘` + `F` | buscar · `Enter` próximo · `Shift+Enter` anterior |
| `Esc` | sai da busca, do foco ou da apresentação |

### Modo apresentação
| Atalho | Ação |
|---|---|
| `P` | entrar / sair |
| `→`, `Espaço`, `PageDown` | revelar o próximo nó |
| `←`, `PageUp` | esconder o último nó revelado |
| `Shift` + `→` | revelar o ramo atual inteiro |
| `1`…`9` | pular para o ramo de nível 1 correspondente |
| `A` | notas automáticas liga/desliga |
| `H` | esconder a interface (fica só o contador "12/87") |
| `Esc` | sair |

`PageDown` e `PageUp` fazem os passadores de slide funcionarem. A edição fica bloqueada durante a apresentação.

### Edição (`E` liga/desliga)
| Atalho | Ação |
|---|---|
| Duplo clique ou `F2` | editar o texto (`Enter` confirma, `Esc` cancela) |
| `Tab` | criar filho |
| `Enter` | criar irmão |
| `Delete` / `Backspace` | apagar (pede confirmação se tiver filhos) |
| `Alt` + `↑` / `↓` | reordenar entre irmãos |
| Arrastar um nó sobre outro | mover para dentro dele |
| `Ctrl/⌘` + `Z` / `Ctrl/⌘` + `Shift` + `Z` | desfazer / refazer (100 passos) |

A tag e a nota se editam no painel lateral, com pré-visualização.

### Arquivo
| Atalho | Ação |
|---|---|
| Exportar ▾ | PNG 2× (mapa inteiro no estado atual), SVG, JSON, Imprimir/PDF |
| `Ctrl/⌘` + `P` | imprimir / salvar PDF (paisagem, ajustado à página) |
| Soltar um `.json` na janela | importar mapa |

Ao importar um JSON com `id` de um mapa existente, o app pede confirmação para substituir o conteúdo (dá para desfazer). Um `id` novo vira um mapa "(importado)" guardado neste navegador.

## Publicar na Vercel

O `vercel.json` faz a raiz do site (`/`) abrir o `mapa-mental.html`. Sem ele, a Vercel procura um `index.html` e dá 404. Não precisa de build: em *Framework Preset*, use **Other** e deixe *Build Command* e *Output Directory* vazios.

A URL `*.vercel.app` de produção é **pública**: qualquer pessoa com o link vê os mapas e as notas. Se o conteúdo for sensível, ative a *Deployment Protection* no projeto da Vercel ou use o arquivo localmente.

## Teste de carga

Abra `mapa-mental.html?stress=250` para adicionar um mapa gerado com 250 nós ao seletor. Troque o número para testar outros tamanhos.

## Limitações conhecidas

- **Tags em 10px.** O pedido era 10px, mas isso contradiz a regra "nada abaixo de 13px". Em call, a compressão do Meet deixa 10px ilegível. Para trocar, mude `--tag-size` no topo do CSS para `13px`.
- **Mapas grandes totalmente abertos não cabem na tela.** Com todos os filhos à direita, 250 nós abertos formam uma coluna de ~7000px. `F` para em 20% sem mostrar tudo. Na call, prefira `Shift+1`/`Shift+2` e abra os ramos conforme fala.
- **Apresentação percorre a árvore inteira**, inclusive ramos recolhidos. Mapas muito grandes podem ter centenas de passos. Use `Shift+→` e `1`–`9` para avançar mais rápido.
- **Edições locais** existem só no navegador onde foram feitas (veja "Cuidado" acima).
