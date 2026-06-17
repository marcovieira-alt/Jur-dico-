# Jur-dico — Servidores MCP do Google Maps

Este projeto tem dois servidores MCP (Model Context Protocol) do Google Maps
instalados e configurados. Ambos dão ao assistente de IA acesso a geocodificação,
busca de lugares, rotas, distâncias e outros dados do Google Maps Platform.

## Servidores disponíveis

| Servidor | Pacote | Ferramentas | Status |
|---|---|---|---|
| **`google-maps`** (recomendado) | [`@cablate/mcp-google-map`](https://github.com/cablate/mcp-google-map) | 18 (geocode, places, rotas, clima, qualidade do ar, mapas estáticos, etc.) | Mantido ativamente |
| **`google-maps-legacy`** | [`@modelcontextprotocol/server-google-maps`](https://www.npmjs.com/package/@modelcontextprotocol/server-google-maps) | 7 (geocode, places, distance matrix, elevation, directions) | ⚠️ Descontinuado |

> O servidor de referência original da Anthropic (`@modelcontextprotocol/server-google-maps`)
> foi **descontinuado** e tem vulnerabilidades sem correção. Ele continua funcional
> e foi mantido aqui como `google-maps-legacy` por compatibilidade, mas prefira o
> servidor `google-maps`.

## Pré-requisitos

1. **Node.js** >= 18
2. Uma **chave de API do Google Maps Platform**.

## Configuração

1. Instale as dependências:

   ```bash
   npm install
   ```

2. Crie sua chave de API no
   [Google Cloud Console](https://console.cloud.google.com/apis/credentials)
   e habilite as APIs necessárias (Geocoding, Places API (New), Directions,
   Distance Matrix, Elevation, Time Zone, Weather, Air Quality).

3. Copie o arquivo de exemplo e preencha a chave:

   ```bash
   cp .env.example .env
   # edite .env e coloque sua GOOGLE_MAPS_API_KEY
   ```

4. Exporte a chave no ambiente onde o cliente MCP roda:

   ```bash
   export GOOGLE_MAPS_API_KEY="sua-chave-aqui"
   ```

## Uso com o Claude Code

O arquivo [`.mcp.json`](./.mcp.json) já registra os dois servidores no escopo do
projeto. Ao abrir o Claude Code nesta pasta, ele detecta a configuração e pede
aprovação para carregar os servidores. Verifique com:

```bash
claude mcp list
```

## Teste rápido (sem cliente MCP)

Liste as ferramentas do servidor recomendado direto pelo terminal:

```bash
echo '{"jsonrpc":"2.0","id":1,"method":"tools/list","params":{}}' \
  | GOOGLE_MAPS_API_KEY="$GOOGLE_MAPS_API_KEY" \
    npx @cablate/mcp-google-map --stdio
```

## Ferramentas (servidor recomendado)

`maps_geocode`, `maps_reverse_geocode`, `maps_batch_geocode`, `maps_search_places`,
`maps_search_nearby`, `maps_place_details`, `maps_distance_matrix`, `maps_directions`,
`maps_plan_route`, `maps_search_along_route`, `maps_elevation`, `maps_timezone`,
`maps_weather`, `maps_air_quality`, `maps_static_map`, `maps_explore_area`,
`maps_compare_places`, `maps_local_rank_tracker`.

Use `GOOGLE_MAPS_ENABLED_TOOLS` para limitar quais ferramentas ficam expostas.
