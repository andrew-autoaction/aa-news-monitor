// Auto Action News Monitor — Cloudflare Worker v2
// Handles: RSS/HTML proxy + Anthropic API relay

const ALLOWED_DOMAINS = [
  'autoaction.com.au',
  'motorsport.com',
  'the-race.com',
  'speedcafe.com',
  'motorsportweek.com',
  'autosport.com',
  'racingnews365.com',
  'jayski.com',
  'crash.net',
  'gpfans.com',
  'motogp.com',
  'wrc.com',
  'fiawec.com',
  'indycar.com',
  'nascar.com',
];

const CORS_HEADERS = {
  'Access-Control-Allow-Origin': '*',
  'Access-Control-Allow-Methods': 'GET, POST, OPTIONS',
  'Access-Control-Allow-Headers': 'Content-Type, x-api-key, anthropic-version',
};

export default {
  async fetch(request) {

    // Handle CORS preflight
    if (request.method === 'OPTIONS') {
      return new Response(null, { headers: CORS_HEADERS });
    }

    const url = new URL(request.url);

    // ─── ROUTE 1: Anthropic API relay (/anthropic) ───────────────────────────
    // The browser can't call Anthropic directly due to CORS.
    // This relay forwards the request server-side and returns the response.
    if (url.pathname === '/anthropic') {
      if (request.method !== 'POST') {
        return new Response('POST only', { status: 405, headers: CORS_HEADERS });
      }

      try {
        const body = await request.text();

        // Your Anthropic API key — set this as a Worker secret (see instructions)
        const apiKey = request.env?.ANTHROPIC_API_KEY || '';

        const response = await fetch('https://api.anthropic.com/v1/messages', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
            'x-api-key': apiKey,
            'anthropic-version': '2023-06-01',
          },
          body,
        });

        const data = await response.text();
        return new Response(data, {
          status: response.status,
          headers: {
            'Content-Type': 'application/json',
            ...CORS_HEADERS,
          },
        });
      } catch (err) {
        return new Response(JSON.stringify({ error: err.message }), {
          status: 502,
          headers: { 'Content-Type': 'application/json', ...CORS_HEADERS },
        });
      }
    }

    // ─── ROUTE 2: RSS / HTML proxy (?url=...) ────────────────────────────────
    if (request.method !== 'GET') {
      return new Response('Method not allowed', { status: 405, headers: CORS_HEADERS });
    }

    const target = url.searchParams.get('url');
    if (!target) {
      return new Response('Missing ?url= parameter', { status: 400, headers: CORS_HEADERS });
    }

    const targetHost = new URL(target).hostname.replace('www.', '');
    const permitted = ALLOWED_DOMAINS.some(d => targetHost.endsWith(d));
    if (!permitted) {
      return new Response(`Domain not permitted: ${targetHost}`, { status: 403, headers: CORS_HEADERS });
    }

    try {
      const response = await fetch(target, {
        headers: {
          'User-Agent': 'Mozilla/5.0 (compatible; AutoActionBot/1.0; +https://autoaction.com.au)',
          'Accept': 'text/html,application/xhtml+xml,application/xml,application/rss+xml;q=0.9,*/*;q=0.8',
          'Accept-Language': 'en-AU,en;q=0.9',
        },
        cf: {
          cacheTtl: 300,
          cacheEverything: true,
        },
      });

      const body = await response.text();
      return new Response(body, {
        status: response.status,
        headers: {
          'Content-Type': response.headers.get('Content-Type') || 'text/plain',
          'Cache-Control': 'public, max-age=300',
          ...CORS_HEADERS,
        },
      });
    } catch (err) {
      return new Response(`Fetch failed: ${err.message}`, {
        status: 502,
        headers: CORS_HEADERS,
      });
    }
  },
};
