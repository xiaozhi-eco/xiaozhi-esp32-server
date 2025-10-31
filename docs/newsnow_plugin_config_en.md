# get_news_from_newsnow Plugin News Source Configuration Guide

## Overview

The `get_news_from_newsnow` plugin now supports dynamic news source configuration through the Web management interface, no longer requiring code modifications. Users can configure different news sources for each agent in the Control Console.

## Configuration Methods

### 1. Configure via Web Management Interface (Recommended)

1. Log in to the Control Console
2. Go to "Role Configuration" page
3. Select the agent you want to configure
4. Click the "Edit Functions" button
5. Find the "newsnow News Aggregation" plugin in the parameter configuration area on the right
6. Enter semicolon-separated Chinese names in the "News Source Configuration" field

### 2. Configuration File Method

Configure in `config.yaml`:

```yaml
plugins:
  get_news_from_newsnow:
    url: "https://newsnow.busiyi.world/api/s?id="
    news_sources: "The Paper;Baidu Hot Search;Caixin;CWeibo;Douyin"
```

## News Source Configuration Format

News source configuration uses semicolon-separated Chinese names in the format:

```
Chinese Name 1;Chinese Name 2;Chinese Name 3
```

### Configuration Example

```
The Paper;Baidu Hot Search;Caixin;CWeibo;Douyin;Zhihu;36Kr
```

## Supported News Sources

The plugin supports the following Chinese names:

- The Paper (澎湃新闻)
- Baidu Hot Search (百度热搜)
- Caixin (财联社)
- CWeibo (微博)
- Douyin (抖音)
- Zhihu (知乎)
- 36Kr (36氪)
- Wallstreetcn (华尔街见闻)
- ITHome (IT之家)
- Jinri Toutiao (今日头条)
- Hupu (虎扑)
- Bilibili (哔哩哔哩)
- Kuaishou (快手)
- Xueqiu (雪球)
- Gelonghui (格隆汇)
- Fabu Financial (法布财经)
- Jin 10 Data (金十数据)
- Niu Ke (牛客)
- Sspai (少数派)
- Juejin (稀土掘金)
- Phoenix Net (凤凰网)
- Chongtuobuluo (虫部落)
- Lianhe Zaobao (联合早报)
- Coolapk (酷安)
- Yingxiang Forum (远景论坛)
- Reference News (参考消息)
- Satellite News Agency (卫星通讯社)
- Baidu Tieba (百度贴吧)
- Kaopu News (靠谱新闻)
- And more...

## Default Configuration

If no news sources are configured, the plugin will use the following default configuration:

```
The Paper;Baidu Hot Search;Caixin
```

## Usage Instructions

1. **Configure News Sources**: Set Chinese names of news sources in the Web interface or configuration file, separated by semicolons
2. **Call Plugin**: Users can say "Report news" or "Get news"
3. **Specify News Source**: Users can say "Report The Paper" or "Get Baidu Hot Search"
4. **Get Details**: Users can say "Introduce this news in detail"

## How It Works

1. The plugin accepts Chinese names as parameters (e.g., "The Paper")
2. According to the configured news source list, converts Chinese names to corresponding English IDs (e.g., "thepaper")
3. Uses the English ID to call the API to get news data
4. Returns news content to the user

## Notes

1. Configured Chinese names must exactly match the names defined in CHANNEL_MAP
2. Configuration changes require restarting the service or reloading the configuration
3. If a configured news source is invalid, the plugin will automatically use the default news sources
4. Use semicolons(;) to separate multiple news sources, do not use Chinese semicolons(；)

