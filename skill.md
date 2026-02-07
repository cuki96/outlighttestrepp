---
name: outlight-analytics
version: 1.0.0
description: Real-time cryptocurrency token call analytics from Telegram and Twitter channels. Track KOL performance, discover trending tokens, and analyze caller win rates for better trading decisions.
---

# Outlight Analytics API

Public, read-only analytics API for cryptocurrency token calls from Telegram and Twitter/X channels. No authentication required.

**Base URL:** `https://prod.api.sauron.outlight.fun`

## Overview

Outlight tracks cryptocurrency token calls from Key Opinion Leaders (KOLs) across Telegram and Twitter/X. This API provides AI agents with real-time data to make informed trading decisions based on:
- Caller performance metrics (win rate, average gains)
- Token call frequency and trends
- Historical performance data
- Proof-of-call transparency

## Endpoints

### GET /api/channels/search

Search for cryptocurrency caller channels (KOLs) by name, username, or keywords.

**Parameters:**

| Param     | Type   | Default | Description                                           |
|-----------|--------|---------|-------------------------------------------------------|
| q         | string | ""      | Search query - channel name, username, or keywords    |
| limit     | int    | 50      | Maximum results to return (1-100)                     |
| timeframe | enum   | 7d      | Time window: `1d`, `3d`, `7d`, `14d`, `30d`, `all`   |

**Response:** `ChannelSearchResponse`

| Field    | Type      | Description                              |
|----------|-----------|------------------------------------------|
| channels | Channel[] | Array of matching channels with metrics |
| count    | int       | Total number of results found            |

**Channel Object:**

| Field              | Type     | Description                                    |
|--------------------|----------|------------------------------------------------|
| channel_id         | string   | Unique channel identifier (e.g., "@CryptoGems")|
| username           | string   | Channel username                               |
| name               | string   | Channel display name                           |
| link               | string   | URL to the channel                             |
| subscriber_count   | int      | Number of subscribers                          |
| profile_photo_url  | string   | Channel avatar URL                             |
| platform           | string   | Platform: "telegram" or "twitter"              |
| win_rate           | string   | Percentage of calls reaching 2x (0-100)        |
| avg_gain           | string   | Average percentage gain across all calls       |
| x5_rate            | string   | Percentage of calls reaching 5x                |
| x10_rate           | string   | Percentage of calls reaching 10x               |
| total_calls        | string   | Number of calls in timeframe                   |
| calls_analyzed     | int      | Total calls used for statistics                |
| last_updated       | datetime | Last update timestamp (ISO 8601)               |
| timeframe_days     | int      | Timeframe in days for the metrics              |

---

### GET /api/channels

List all tracked channels with sorting and filtering options.

**Parameters:**

| Param         | Type   | Default          | Description                                        |
|---------------|--------|------------------|----------------------------------------------------|
| sort          | enum   | subscriber_count | Sort by: `subscriber_count`, `win_rate`, `avg_gain`, `x5_rate`, `x10_rate`, `calls_analyzed`, `last_updated`, `total_calls` |
| order         | enum   | desc             | Sort order: `asc` or `desc`                        |
| page          | int    | 1                | Page number (1-based)                              |
| limit         | int    | 20               | Results per page (1-100)                           |
| timeframe     | enum   | 7d               | Time window: `1d`, `3d`, `7d`, `14d`, `30d`, `all`|
| platform      | string | null             | Filter by platform: "telegram" or "twitter"        |
| x2_rate_from  | number | null             | Minimum 2x rate filter                             |
| x2_rate_to    | number | null             | Maximum 2x rate filter                             |
| x5_rate_from  | number | null             | Minimum 5x rate filter                             |
| x5_rate_to    | number | null             | Maximum 5x rate filter                             |
| x10_rate_from | number | null             | Minimum 10x rate filter                            |
| x10_rate_to   | number | null             | Maximum 10x rate filter                            |
| avg_ath_from  | number | null             | Minimum average ATH (all-time high) filter         |
| avg_ath_to    | number | null             | Maximum average ATH filter                         |

**Response:** `ChannelListResponse`

| Field      | Type      | Description                       |
|------------|-----------|-----------------------------------|
| channels   | Channel[] | Array of channel objects          |
| pagination | object    | Pagination metadata               |

**Pagination Object:**

| Field        | Type | Description                  |
|--------------|------|------------------------------|
| page         | int  | Current page number          |
| limit        | int  | Results per page             |
| total        | int  | Total number of channels     |
| totalPages   | int  | Total number of pages        |

---

### GET /api/channels/:channelId/info

Get detailed information and statistics for a specific channel.

**Parameters:**

| Param      | Type   | Default | Description                                         |
|------------|--------|---------|-----------------------------------------------------|
| channelId  | string | -       | **Required.** Channel ID (with or without @ prefix)|
| timeframe  | enum   | 7d      | Time window: `1d`, `3d`, `7d`, `14d`, `30d`, `all` |
| refresh    | bool   | false   | Force cache refresh                                 |

**Response:** `ChannelInfo`

Same as Channel object with additional detailed statistics.

---

### GET /api/channels/:channelId/calls

Get all token calls made by a specific channel with pagination and sorting.

**Parameters:**

| Param      | Type   | Default   | Description                                                    |
|------------|--------|-----------|----------------------------------------------------------------|
| channelId  | string | -         | **Required.** Channel ID (with or without @ prefix)            |
| page       | int    | 1         | Page number                                                    |
| limit      | int    | 20        | Results per page (1-100)                                       |
| sort       | enum   | called_at | Sort by: `called_at`, `highest_fdv`, `current_fdv`, `fdv_at_call`, `current_gain`, `highest_gain` |
| order      | enum   | desc      | Sort order: `asc` or `desc`                                    |

**Response:** `ChannelCallsResponse`

| Field      | Type        | Description                  |
|------------|-------------|------------------------------|
| calls      | TokenCall[] | Array of token call objects  |
| pagination | object      | Pagination metadata          |

**TokenCall Object:**

| Field             | Type     | Description                                        |
|-------------------|----------|----------------------------------------------------|
| token_address     | string   | Solana token contract address                      |
| token_name        | string   | Token name                                         |
| token_symbol      | string   | Token ticker symbol                                |
| called_at         | datetime | When the call was made (ISO 8601)                  |
| fdv_at_call       | string   | Fully Diluted Valuation when called (USD)          |
| current_fdv       | string   | Current FDV (USD)                                  |
| highest_fdv       | string   | Highest FDV reached (USD)                          |
| current_gain      | string   | Current gain percentage                            |
| highest_gain      | string   | Highest gain percentage reached                    |
| price_at_call     | string   | Token price when called                            |
| current_price     | string   | Current token price                                |
| highest_price     | string   | Highest price reached                              |
| message_link      | string   | Link to original call message (proof-of-call)      |
| is_first_call     | bool     | Whether this was the first call for this token     |

---

### GET /api/channels-stats

Get aggregated statistics across all monitored channels.

**Parameters:**

| Param     | Type | Default | Description                                         |
|-----------|------|---------|-----------------------------------------------------|
| timeframe | enum | 7d      | Time window: `1d`, `3d`, `7d`, `14d`, `30d`, `all` |

**Response:** `ChannelsStatsResponse`

| Field              | Type   | Description                                    |
|--------------------|--------|------------------------------------------------|
| total_channels     | int    | Total number of monitored channels             |
| total_calls        | int    | Total token calls in timeframe                 |
| avg_win_rate       | string | Average win rate across all channels           |
| avg_gain           | string | Average gain across all channels               |
| top_performing     | int    | Number of channels with >50% win rate          |
| timeframe_days     | int    | Timeframe in days                              |

---

### GET /api/tokens/search

Search for tokens by contract address, name, or symbol.

**Parameters:**

| Param | Type   | Default | Description                                  |
|-------|--------|---------|----------------------------------------------|
| q     | string | ""      | **Required.** Search query (address/name/symbol) |
| limit | int    | 50      | Maximum results (1-100)                      |

**Response:** `TokenSearchResponse`

| Field  | Type    | Description                     |
|--------|---------|---------------------------------|
| tokens | Token[] | Array of matching token objects |
| count  | int     | Total number of results         |

**Token Object:**

| Field            | Type     | Description                              |
|------------------|----------|------------------------------------------|
| token_address    | string   | Solana contract address                  |
| token_name       | string   | Token name                               |
| token_symbol     | string   | Token symbol                             |
| current_price    | string   | Current price                            |
| current_fdv      | string   | Current Fully Diluted Valuation (USD)    |
| total_calls      | int      | Number of times called                   |
| first_called_at  | datetime | When first called (ISO 8601)             |
| last_called_at   | datetime | Most recent call (ISO 8601)              |
| avg_gain         | string   | Average gain across all calls            |
| best_performing_caller | string | Channel with best performance on this token |

---

### GET /api/tokens/recent

Get recently called tokens sorted by call time.

**Parameters:**

| Param | Type | Default | Description                  |
|-------|------|---------|------------------------------|
| limit | int  | 20      | Maximum results (1-100)      |
| page  | int  | 1       | Page number                  |

**Response:** `TokenListResponse`

| Field      | Type    | Description              |
|------------|---------|--------------------------|
| tokens     | Token[] | Array of token objects   |
| pagination | object  | Pagination metadata      |

---

### GET /api/tokens/most-called

Get tokens sorted by call frequency (most called tokens).

**Parameters:**

| Param     | Type | Default | Description                                         |
|-----------|------|---------|-----------------------------------------------------|
| limit     | int  | 20      | Maximum results (1-100)                             |
| page      | int  | 1       | Page number                                         |
| timeframe | enum | 7d      | Time window: `1d`, `3d`, `7d`, `14d`, `30d`, `all` |

**Response:** `TokenListResponse`

Same as `/api/tokens/recent`.

---

### GET /api/tokens/:tokenAddress

Get detailed information about a specific token including all calls.

**Parameters:**

| Param        | Type   | Default | Description                           |
|--------------|--------|---------|---------------------------------------|
| tokenAddress | string | -       | **Required.** Solana contract address |

**Response:** `TokenDetails`

| Field         | Type        | Description                           |
|---------------|-------------|---------------------------------------|
| token         | Token       | Token object with detailed info       |
| calls         | TokenCall[] | All calls for this token              |
| callers       | Channel[]   | All channels that called this token   |
| performance   | object      | Performance statistics                |

**Performance Object:**

| Field          | Type   | Description                        |
|----------------|--------|------------------------------------|
| total_calls    | int    | Total number of calls              |
| avg_gain       | string | Average gain percentage            |
| median_gain    | string | Median gain percentage             |
| best_gain      | string | Best gain achieved                 |
| worst_gain     | string | Worst performance                  |
| x2_rate        | string | Percentage reaching 2x             |
| x5_rate        | string | Percentage reaching 5x             |
| x10_rate       | string | Percentage reaching 10x            |

---

## Response Format

All endpoints return JSON responses. Successful responses contain data directly or wrapped in a response object. Errors return standard HTTP status codes with error details.

**Success Response Example:**

```json
{
  "channels": [...],
  "count": 42
}
```

**Error Response Example:**

```json
{
  "error": "Invalid parameter",
  "message": "Limit must be between 1 and 100",
  "statusCode": 400
}
```

---

## Data Types

- **datetime**: ISO 8601 format (e.g., `2026-01-15T12:00:00.000Z`)
- **string (numeric)**: Numeric values returned as strings to preserve precision (e.g., `"12345.67"`)
- **enum**: Fixed set of allowed values

---

## Key Metrics Explained

**Win Rate:** Percentage of calls that reached at least 2x from the call price within the timeframe.

**Average Gain:** Mean percentage gain across all calls, calculated using highest price reached for wins, current price for active/neutral calls.

**X5/X10 Rate:** Percentage of calls that reached 5x or 10x respectively.

**FDV (Fully Diluted Valuation):** Market cap if all tokens were in circulation, important for understanding true token size.

**Calls Analyzed:** Minimum 5 calls required for statistics to be meaningful.

---

## Enums

### Timeframe

| Value | Description           |
|-------|-----------------------|
| 1h    | Last 1 hour           |
| 3h    | Last 3 hours          |
| 1d    | Last 24 hours         |
| 3d    | Last 3 days           |
| 7d    | Last 7 days (default) |
| 14d   | Last 14 days          |
| 30d   | Last 30 days          |
| all   | All time              |

### Platform

| Value    | Description |
|----------|-------------|
| telegram | Telegram    |
| twitter  | Twitter/X   |

---

## Rate Limits

Public API with fair usage rate limits:
- **100 requests per minute** per IP address
- **1000 requests per hour** per IP address

Rate limit headers are included in responses:
- `X-RateLimit-Limit`: Maximum requests allowed
- `X-RateLimit-Remaining`: Requests remaining
- `X-RateLimit-Reset`: Unix timestamp when limit resets

---

## Caching

Responses are cached server-side for performance:
- Channel search and lists: **5 minutes**
- Channel details and calls: **2 minutes**
- Token data: **1 minute**
- Statistics: **5 minutes**

Cache headers:
- `X-Cache`: `HIT` or `MISS`
- `Cache-Control`: Caching directives

---

## Use Cases for AI Trading Agents

### 1. Discover High-Performance Callers
```
GET /api/channels?sort=win_rate&order=desc&limit=10&timeframe=7d
```
Find channels with the best win rates to follow their calls.

### 2. Monitor Recent Token Calls
```
GET /api/tokens/recent?limit=20
```
Get the latest token calls to identify early opportunities.

### 3. Track Trending Tokens
```
GET /api/tokens/most-called?timeframe=1d&limit=10
```
Discover which tokens are getting the most attention from callers.

### 4. Analyze Specific Caller Performance
```
GET /api/channels/@CryptoGems/calls?sort=highest_gain&order=desc
```
Review historical performance of a specific caller.

### 5. Research Token Before Trading
```
GET /api/tokens/{tokenAddress}
```
Get comprehensive data about a token including all calls and performance metrics.

---

## Best Practices

1. **Use appropriate timeframes**: For day trading, use `1d` or `3d`. For swing trading, use `7d` or `14d`.

2. **Check win rates**: Prioritize channels with win_rate > 50% and sufficient calls_analyzed (> 10).

3. **Verify proof-of-call**: Use `message_link` to verify the original call and timing.

4. **Consider first calls**: Tokens with `is_first_call: true` may offer better entry opportunities.

5. **Monitor multiple metrics**: Don't rely on win rate alone - check avg_gain, x5_rate, and x10_rate.

6. **Respect rate limits**: Implement exponential backoff for rate limit errors.

7. **Cache responses**: Use cache headers to minimize redundant requests.

---

## Example Workflows

### Finding Alpha Callers
```
1. GET /api/channels?sort=win_rate&order=desc&timeframe=7d&limit=20
2. Filter channels with calls_analyzed >= 10
3. GET /api/channels/{top_channel}/calls to review performance
4. Monitor recent calls from top performers
```

### Token Discovery Pipeline
```
1. GET /api/tokens/most-called?timeframe=1d
2. For interesting tokens: GET /api/tokens/{address}
3. Check performance metrics and caller quality
4. Review proof-of-call links for context
5. Make informed trading decision
```

### Real-Time Monitoring
```
1. GET /api/tokens/recent?limit=50 (every 1-2 minutes)
2. Filter for is_first_call: true
3. Check caller's win_rate from /api/channels/{caller}/info
4. Execute trades on high-quality signals
```

---

## Support

For API issues, feature requests, or integration support:
- Documentation: https://outlightfun.mintlify.app/
- Website: https://outlight.fun/
- Twitter: @OutlightAI

---

## Changelog

### Version 1.0.0
- Initial public API release
- Channel search and listing
- Token search and tracking
- Performance analytics
- Proof-of-call transparency