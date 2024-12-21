<div align="center">
    <img width="400" src="img/slogan.jpg" alt="logo"><br>

# 闲鱼二手99新女生自用 Clash 规则
</div>

<img decoding="async" align=right src="img/laoshu.gif" width="35%">

- 基于 mimoho 内核 1.8.10 版本，理论支持绝大部分第三方 mihomo gui 客户端

- 如果遇到了问题，欢迎在 [issues](https://github.com/Kirby-of-the-Stars/JMusicKook/issues) 中反馈


> [!CAUTION]
> 本项目仅供爱好者学习使用,您将承担所有的法律责任, 作者与其他贡献者将不承担任何责任.


> [!IMPORTANT]
> 本人使用的是 mimoho 内核，所以绝大部分内容都是基于 mihomo 内核进行的配置

## 覆写配置

> [!TIP]
> 由于配置文件内写了非常多的注释
>
> 理论上不需要重复赘述过多的内容，根据文本内的注释自行理解即可
```js
// Define the `main` function

const proxyName = "代理模式";

function main(params) {
    if (!params.proxies) return params;
    overwriteProxyGroups(params);
    overwriteRules(params);
    overwriteDns(params);
    overwriteSniffer(params);
    overwriteHosts(params);
    overwriteBasicOptions(params);
    overwriteTunnel(params);
    return params;
}

// 覆写Basic Options
function overwriteBasicOptions (params) {
    const otherOptions = {
        "mixed-port": 7890,
        "allow-lan": true,
        "unified-delay": true,
        "tcp-concurrent": true,
        "find-process-mode": "strict",
        "global-client-fingerprint": "chrome",
        profile: {
            "store-selected": true,
            "store-fake-ip": true,
        },
        ipv6: false,
        mode: "rule",
        udp: true,
    };
    Object.keys (otherOptions).forEach ((key) => {
        params [key] = otherOptions [key];
    });
}

// 覆写hosts
function overwriteHosts(params) {
    const hosts = {
        "dns.alidns.com": [
            "223.5.5.5",
            "223.6.6.6",
            "2400:3200:baba::1",
            "2400:3200::1",
        ],
        "doh.pub": ["120.53.53.53", "1.12.12.12"],
    };
    params.hosts = hosts;
}

function overwriteSniffer(params) {
    const snifferConfig = {
        enable: true,
        "force-dns-mapping": true,
        "parse-pure-ip": true,
        "override-destination": false,

        sniff: {
            HTTP: {
                ports: ["80", "8080-8880"],
                "override-destination": false,
            },

            TLS: {
                ports: ["443", "8443"],
            },

            QUIC: {
                ports: ["443", "8443"],
            },
        },

        // 强制嗅探结果
        "force-domain": ["google.com", "+.v2ex.com"],

        // 跳过嗅探结果
        "skip-domain": ["Mijia Cloud", "+.apple.com"],
    };

    params["sniffer"] = snifferConfig;
}

// 覆写代理组
function overwriteProxyGroups(params) {
    // 添加自用代理
    params.proxies
        .push
        //  { name: '1 - 香港 - 示例 ', type: *, server: **, port: *, cipher: **, password: **, udp: true }
        ();
    // 自动选择代理组，按地区分组选延迟最低
    const countryRegions = [
        {
            code: "HK",
            name: "🇭🇰 香港",
            icon: "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/flags/hk.svg",
            regex: /(香港|HK|Hong Kong|🇭🇰)/i,
        },
        {
            code: "TW",
            name: "🇹🇼 台湾",
            icon: "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/flags/tw.svg",
            regex: /(台湾|TW|Taiwan|🇹🇼)/i,
        },
        {
            code: "SG",
            name: "🇸🇬 新加坡",
            icon: "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/flags/sg.svg",
            regex: /(新加坡|狮城|SG|Singapore|🇸🇬)/i,
        },
        {
            code: "AR",
            name: "🇦🇷 阿根廷",
            icon: "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/flags/ar.svg",
            regex: /(阿根廷|AR|Argentina|🇦🇷)/i,
        },
        {
            code: "JP",
            name: "🇯🇵 日本",
            icon: "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/flags/jp.svg",
            regex: /(日本|JP|Japan|🇯🇵)/i,
        },
        {
            code: "US",
            name: "🇺🇸 美国",
            icon: "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/flags/us.svg",
            regex: /(美国|US|USA|United States|America|🇺🇸)/i,
        },
        {
            code: "DE",
            name: "🇩🇪 德国",
            icon: "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/flags/de.svg",
            regex: /(德国|DE|Germany|🇩🇪)/i,
        },
        {
            code: "KR",
            name: "🇰🇷 韩国",
            icon: "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/flags/kr.svg",
            regex: /(韩国|KR|Korea|South Korea|🇰🇷)/i,
        },
        {
            code: "UK",
            name: "🇬🇧 英国",
            icon: "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/flags/gb.svg",
            regex: /(英国|UK|United Kingdom|Britain|Great Britain|🇬🇧)/i,
        },
        {
            code: "CA",
            name: "🇨🇦 加拿大",
            icon: "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/flags/ca.svg",
            regex: /(加拿大|CA|Canada|🇨🇦)/i,
        },
        {
            code: "AU",
            name: "🇦🇺 澳大利亚",
            icon: "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/flags/au.svg",
            regex: /(澳大利亚|AU|Australia|🇦🇺)/i,
        },
        {
            code: "ES",
            name: "🇪🇸 西班牙",
            icon: "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/flags/es.svg",
            regex: /\b(西班牙|ES|Spain|🇪🇸)\b/i,
        },
        {
            code: "NL",
            name: "🇳🇱 荷兰",
            icon: "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/flags/nl.svg",
            regex: /\b(荷兰|NL|Netherlands|🇳🇱)\b/i,
        },
        {
            code: "TR",
            name: "🇹🇷 土耳其",
            icon: "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/flags/tr.svg",
            regex: /(土耳其|TR|Turkey|🇹🇷)/i,
        },
        {
            code: "RU",
            name: "🇷🇺 俄罗斯",
            icon: "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/flags/ru.svg",
            regex: /(俄罗斯|RU|Russia|🇷🇺)/i,
        },
        {
            code: "IN",
            name: "🇮🇳 印度",
            icon: "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/flags/in.svg",
            regex: /\b(印度|IN|India|🇮🇳)\b/i,
        },
        {
            code: "BR",
            name: "🇧🇷 巴西",
            icon: "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/flags/br.svg",
            regex: /(巴西|BR|Brazil|🇧🇷)/i,
        },
        {
            code: "IT",
            name: "🇮🇹 意大利",
            icon: "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/flags/it.svg",
            regex: /(意大利|IT|Italy|🇮🇹)/i,
        },
        {
            code: "CH",
            name: "🇨🇭 瑞士",
            icon: "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/flags/ch.svg",
            regex: /(瑞士|CH|Switzerland|🇨🇭)/i,
        },
        {
            code: "SE",
            name: "🇸🇪 瑞典",
            icon: "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/flags/se.svg",
            regex: /(瑞典|SE|Sweden|🇸🇪)/i,
        },
        {
            code: "NO",
            name: "🇳🇴 挪威",
            icon: "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/flags/no.svg",
            regex: /(挪威|NO|Norway|🇳🇴)/i,
        },
        {
            name: "其它 - 自动选择",
            regex: /(?!.*(?: 剩余 | 到期 | 主页 | 官网 | 游戏 | 关注))(.*)/,
        },
    ];

    // 所有代理
    // 所有地区
    const allRegex =
        /^(?!.*(?:自动|故障|流量|官网|套餐|机场|订阅|年|月|失联|频道|Traffic|Expire)).*$/;
    const allProxies = getProxiesByRegexOne(params, allRegex);
    // const allProxies = params["proxies"].map((e) => e.name);

    const availableCountryCodes = new Set();
    const otherProxies = [];
    for (const proxy of params["proxies"]) {
        let found = false;
        for (const region of countryRegions) {
            if (region.regex.test(proxy.name)) {
                availableCountryCodes.add(region.name);
                found = true;
                break;
            }
        }
        if (!found) {
            otherProxies.push(proxy.name);
        }
    }

    const autoProxyGroupRegexs = countryRegions
        .filter((region) => availableCountryCodes.has(region.name))
        .map((region) => ({
            name: `${region.name} - 自动选择`,
            regex: region.regex,
        }));

    const autoProxyGroups = autoProxyGroupRegexs
        .map((item) => ({
            name: item.name,
            type: "fallback",
            url: "http://www.gstatic.com/generate_204",
            interval: 300,
            tolerance: 50,
            proxies: getProxiesByRegex(params, item.regex),
            hidden: true,
        }))
        .filter((item) => item.proxies.length > 0);

    const manualProxyGroupsConfig = countryRegions
        .filter((region) => availableCountryCodes.has(region.name))
        .map((region) => ({
            name: `${region.name} - 手动选择`,
            type: "select",
            proxies: getManualProxiesByRegex(params, region.regex),
            icon: region.icon,
            hidden: false,
        }))
        .filter((item) => item.proxies.length > 0);

    const groups = [
        {
            name: proxyName,
            type: "select",
            url: "http://www.gstatic.com/generate_204",
            icon: "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/adjust.svg",
            proxies: [
                "延迟优选",
                "故障转移",
                "手动选择",
                "负载均衡 (散列)",
                "负载均衡 (轮询)",
                "DIRECT",
            ],
        },
        {
            name: "延迟优选",
            type: "url-test",
            icon: "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/speed.svg",
            "exclude-filter": "自动选择|手动选择",
            proxies: allProxies.length > 0 ? allProxies : ["DIRECT"],
            hidden: true,
        },
        {
            name: "故障转移",
            type: "fallback",
            icon: "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/ambulance.svg",
            "exclude-filter": "自动选择|手动选择",
            proxies: allProxies.length > 0 ? allProxies : ["DIRECT"],
            hidden: true,
        },
        {
            name: "手动选择",
            type: "select",
            icon: "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/link.svg",
            "exclude-filter": "自动选择",
            proxies: [
                ...countryRegions
                    .filter((region) => availableCountryCodes.has(region.name))
                    .flatMap((region) => [
                        `${region.name} - 手动选择`,
                    ]),
            ],
        },
        {
            name: "负载均衡 (散列)",
            type: "load-balance",
            url: "http://www.gstatic.com/generate_204",
            icon: "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/balance.svg",
            interval: 300,
            "max-failed-times": 3,
            strategy: "consistent-hashing",
            lazy: true,
            "exclude-filter": "自动选择|手动选择",
            proxies: allProxies.length > 0 ? allProxies : ["DIRECT"],
            hidden: true,
        },
        {
            name: "负载均衡 (轮询)",
            type: "load-balance",
            url: "http://www.gstatic.com/generate_204",
            icon: "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/merry_go.svg",
            interval: 300,
            "max-failed-times": 3,
            "exclude-filter": "自动选择|手动选择",
            strategy: "round-robin",
            lazy: true,
            proxies: allProxies.length > 0 ? allProxies : ["DIRECT"],
            hidden: true,
        },
        {
            name: "电报消息",
            type: "select",
            proxies: [
                proxyName,
                ...countryRegions
                    .filter((region) => availableCountryCodes.has(region.name))
                    .flatMap((region) => [
                        `${region.name} - 自动选择`,
                        `${region.name} - 手动选择`,
                    ]),
                "DIRECT",
            ],
            // "include-all": true,
            icon: "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/telegram.svg",
        },
        {
            name: "AI",
            type: "select",
            proxies: [
                proxyName,
                ...countryRegions
                    .filter((region) => availableCountryCodes.has(region.name))
                    .flatMap((region) => [
                        `${region.name} - 自动选择`,
                        `${region.name} - 手动选择`,
                    ]),
                "DIRECT",
            ],
            // "include-all": true,
            icon: "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/chatgpt.svg",
        },
        {
            name: "流媒体",
            type: "select",
            proxies: [
                proxyName,
                ...countryRegions
                    .filter((region) => availableCountryCodes.has(region.name))
                    .flatMap((region) => [
                        `${region.name} - 自动选择`,
                        `${region.name} - 手动选择`,
                    ]),
                "DIRECT",
            ],
            // "include-all": true,
            icon: "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/youtube.svg",
        },
        {
            name: "苹果服务",
            type: "select",
            proxies: [
                proxyName,
                ...countryRegions
                    .filter((region) => availableCountryCodes.has(region.name))
                    .flatMap((region) => [
                        `${region.name} - 自动选择`,
                        `${region.name} - 手动选择`,
                    ]),
            ],
            // "include-all": true,
            icon: "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/apple.svg",
        },
        {
            name: "微软服务",
            type: "select",
            proxies: [
                proxyName,
                ...countryRegions
                    .filter((region) => availableCountryCodes.has(region.name))
                    .flatMap((region) => [
                        `${region.name} - 自动选择`,
                        `${region.name} - 手动选择`,
                    ]),
            ],
            // "include-all": true,
            icon: "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/microsoft.svg",
        },
        {
            name: "漏网之鱼",
            type: "select",
            proxies: ["DIRECT", proxyName],
            icon: "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/fish.svg",
        },
    ];

    autoProxyGroups.length &&
    groups[2].proxies.unshift(...autoProxyGroups.map((item) => item.name));
    groups.push(...autoProxyGroups);
    groups.push(...manualProxyGroupsConfig);
    params["proxy-groups"] = groups;
}

// 修改规则
function overwriteRules(params) {
    const customRules = [
        // 在此添加自定义规则，优先级次于ad。例子：
        // "DOMAIN,baidu.com,DIRECT",
    ];

    // 广告拦截 / 隐私保护 / Malware 拦截 / Phiishing 拦截
    const adNonipRules = [
        "RULE-SET,Reject_no_ip,REJECT",
        "RULE-SET,Reject_domainset,REJECT",
        "RULE-SET,Reject_no_ip_drop,REJECT-DROP",
        "RULE-SET,Reject_no_ip_no_drop,REJECT",
    ];

    const nonipRules = [
        // 个人遇到需要代理的域名(比较特殊)
        "RULE-SET,CustomProxy_no_ip," + proxyName,

        // GoolgeFCM 推送
        "RULE-SET,GoogleFCM_no_ip,DIRECT",

        // 网易云音乐
        "RULE-SET,NetEaseMusic_no_ip,DIRECT",

        // SteamCN
        "RULE-SET,SteamCN_no_ip,DIRECT",
        // Steam
        "RULE-SET,Steam_no_ip," + proxyName,

        /**
         * 包含所有常见静态资源 CDN 域名、对象存储域名
         * 如果你正在使用商业性质的公共代理服务、且你的服务商提供按低倍率结算流量消耗的节点，可使用上述规则组将流量分配给这部分节点
         */
        "RULE-SET,CDN_domainset," + proxyName,
        "RULE-SET,CDN_no_ip," + proxyName,

        // 流媒体域名
        /**
         * 包含
         * 4gtv、AbemaTV、All4、Amazon Prime Video、Apple TV、Apple Music TV、Bahamut、BBC、Bilibili Intl、
         * DAZN、Deezer、Disney+、Discovery+、DMM、encoreTVB、Fox Now、Fox+、HBO GO/Now/Max/Asia、Hulu、HWTV、
         * JOOX、Jwplayer、KKBOX、KKTV、Line TV、Naver TV、myTV Super、Netflix、niconico、Now E、Paramount+、PBS、Peacock、Pandora、PBS、Pornhub、SoundCloud、
         * PBS、Spotify、TaiwanGood、Tiktok Intl、Twitch、ViuTV、ShowTime、iQiYi Global、Himalaya Podcast、Overcast、WeTV
         */
        "RULE-SET,Stream_no_ip,流媒体",

        // tg 消息
        /**
         * 推荐仅使用 IP CIDR 规则。IP CIDR 规则数据完全来自 Telegram 官方发布的 CIDR 列表，不包含 Telegram 尚未启用的 CDN、数据中心的 IP。
         * ASN 规则仅适合作为补充；搭配非官方 MaxMind GeoLite 数据库（例如 GeoIP2-CN）使用时会影响匹配。
         */
        "RULE-SET,Telegram_no_ip,电报消息",

        // 云上贵州（CN）的苹果 CDN 无特殊需求直连即可
        "RULE-SET,AppleCDN_no_ip,DIRECT",
        // 苹果 CN 域名
        "RULE-SET,AppleCN_no_ip,DIRECT",

        // Microsoft 中国 CDN
        "RULE-SET,MicrosoftCDN_no_ip,DIRECT",

        // 软件更新、操作系统等大文件下载
        /**
         * 这部分域名可能包含 Microsoft 和 Apple 的国内 CDN 节点
         * 如果你设置了前面的Microsoft 和 Apple 的国内 CDN 节点为直连，按照优先级这部分CDN不会被代理，请放心
         */
        "RULE-SET,Download_domainset," + proxyName,
        "RULE-SET,Download_no_ip," + proxyName,

        // 苹果需要代理的域名
        "RULE-SET,Apple_no_ip,苹果服务",

        // 微软需要代理域名
        "RULE-SET,Microsoft_no_ip,微软服务",

        // ai 相关
        /**
         * 包含 OpenAI、Google Gemini、Claude、Perplexity 等
         */
        "RULE-SET,AI_no_ip,AI",

        // 常见海外服务和互联网公司的域名 有部分域名被DNS污染，故使用代理
        "RULE-SET,Global_no_ip," + proxyName,

        // 国内常见互联网公司和服务的域名
        "RULE-SET,Domestic_no_ip,DIRECT",
        "RULE-SET,Direct_no_ip,DIRECT",

        // 内网域名和局域网 IP
        /**
         * 域名列表包含 .local 和局域网 IP 的 in-addr.arpa 域名（即 AS112 域名）
         * 这部分域名一般会被解析到局域网 IP、需要走内网 DNS 解析、需要直连访问
         */
        "RULE-SET,Lan_no_ip,DIRECT",
    ];

    const ipRules = [
        // GooleFCM 推送
        "RULE-SET,GoogleFCM_ip,DIRECT",

        // 网易云音乐
        "RULE-SET,NetEaseMusic_ip,DIRECT",

        // SteamCN ip
        "RULE-SET,SteamCN_ip,DIRECT",

        // 广告拦截 / 隐私保护 / Malware 拦截 / Phiishing 拦截（ip）
        "RULE-SET,Reject_ip,REJECT",

        // telegram ip
        "RULE-SET,Telegram_ip,电报消息",

        // 流媒体 ip
        /**
         * 包含
         * 4gtv、AbemaTV、All4、Amazon Prime Video、Apple TV、Apple Music TV、Bahamut、BBC、Bilibili Intl、
         * DAZN、Deezer、Disney+、Discovery+、DMM、encoreTVB、Fox Now、Fox+、HBO GO/Now/Max/Asia、Hulu、HWTV、
         * JOOX、Jwplayer、KKBOX、KKTV、Line TV、Naver TV、myTV Super、Netflix、niconico、Now E、Paramount+、PBS、Peacock、Pandora、PBS、Pornhub、SoundCloud、
         * PBS、Spotify、TaiwanGood、Tiktok Intl、Twitch、ViuTV、ShowTime、iQiYi Global、Himalaya Podcast、Overcast、WeTV
         */
        "RULE-SET,Stream_ip,流媒体",

        // 国内常见互联网公司和服务的 IP
        "RULE-SET,Domestic_ip,DIRECT",
        "RULE-SET,China_ip,DIRECT",

        // 内网域名和局域网 IP
        /**
         * 域名列表包含 .local 和局域网 IP 的 in-addr.arpa 域名（即 AS112 域名）
         * 这部分域名一般会被解析到局域网 IP、需要走内网 DNS 解析、需要直连访问
         */
        "RULE-SET,Lan_ip,DIRECT",

        // 兜底
        "MATCH,漏网之鱼",
    ];

    const allNonipRules = [...adNonipRules, ...customRules, ...nonipRules];

    // 规则
    // 需要非IP类规则写在 IP类规则之前！
    /**
     * 避免 DNS 污染和 DNS 泄漏最有效的办法就是永远不在本地进行 DNS 解析，而 Mihomo 能且只能通过 Fake IP 和域名规则匹配的方式 可以实现非直连域名 一定不在本地本机进行任何 DNS 解析。
     * 在 Mihomo 中，规则自上而下匹配，只有当遇到 IP 类规则（如 IP-CIDR、IP-CIDR6、GEOIP 和 IP-ASN）时才会发起 DNS 解析。
     * 因此，在 Mihomo 中，将会触发 DNS 解析的规则放在域名和 URL 匹配规则后面非常重要。
     */
    const rules = [
        // 非ip类规则
        ...allNonipRules,

        // ip类规则
        ...ipRules,
    ];

    // 插入规则
    params.rules = rules;

    // 远程规则类型
    const ruleAnchor = {
        ip: {
            type: "http",
            interval: 1800,
            behavior: "ipcidr",
            format: "yaml",
        },
        domain: {
            type: "http",
            interval: 1800,
            behavior: "domain",
            format: "yaml",
        },
        classical: {
            type: "http",
            interval: 1800,
            behavior: "classical",
            format: "yaml",
        },
    };

    // 自己仓库的规则
    const ruleProviders = {
        /**
         * 屏蔽部分
         */

        // ##################################################################################################################

        // 广告拦截 / 隐私保护 / Malware 拦截 / Phiishing 拦截
        Reject_ip: {
            ...ruleAnchor.classical,
            url: "https://raw.githubusercontent.com/RealSeek/Clash_Rule_DIY/refs/heads/mihomo/REJECT/ip/Reject_ip.yaml",
            path: "./ruleset/RealSeek/Clash_Rule_DIY/REJECT/ip/Reject_ip.yaml",
        },

        // ##################################################################################################################

        // 广告拦截 / 隐私保护 / Malware 拦截 / Phiishing 拦截
        Reject_no_ip: {
            ...ruleAnchor.classical,
            url: "https://raw.githubusercontent.com/RealSeek/Clash_Rule_DIY/refs/heads/mihomo/REJECT/no_ip/Reject_no_ip.yaml",
            path: "./ruleset/RealSeek/Clash_Rule_DIY/REJECT/no_ip/Reject_no_ip.yaml",
        },

        Reject_domainset: {
            ...ruleAnchor.domain,
            url: "https://raw.githubusercontent.com/RealSeek/Clash_Rule_DIY/refs/heads/mihomo/REJECT/no_ip/Reject_domainset.yaml",
            path: "./ruleset/RealSeek/Clash_Rule_DIY/REJECT/no_ip/Reject_domainset.yaml",
        },

        Reject_no_ip_drop: {
            ...ruleAnchor.classical,
            url: "https://raw.githubusercontent.com/RealSeek/Clash_Rule_DIY/refs/heads/mihomo/REJECT/no_ip/Reject_no_ip_drop.yaml",
            path: "./ruleset/RealSeek/Clash_Rule_DIY/REJECT/no_ip/Reject_no_ip_drop.yaml",
        },

        Reject_no_ip_no_drop: {
            ...ruleAnchor.classical,
            url: "https://raw.githubusercontent.com/RealSeek/Clash_Rule_DIY/refs/heads/mihomo/REJECT/no_ip/Reject_no_ip_no_drop.yaml",
            path: "./ruleset/RealSeek/Clash_Rule_DIY/REJECT/no_ip/Reject_no_ip_no_drop.yaml",
        },

        // ##################################################################################################################

        /**
         * 直连部分
         */

        // ##################################################################################################################

        // 国内常见互联网公司和服务的 IP
        China_ip: {
            ...ruleAnchor.ip,
            url: "https://raw.githubusercontent.com/RealSeek/Clash_Rule_DIY/refs/heads/mihomo/DIRECT/ip/China_ip.yaml",
            path: "./ruleset/RealSeek/Clash_Rule_DIY/DIRECT/ip/China_ip.yaml",
        },

        // 国内常见互联网公司和服务的 IP
        Domestic_ip: {
            ...ruleAnchor.classical,
            url: "https://raw.githubusercontent.com/RealSeek/Clash_Rule_DIY/refs/heads/mihomo/DIRECT/ip/Domestic_ip.yaml",
            path: "./ruleset/RealSeek/Clash_Rule_DIY/DIRECT/ip/Domestic_ip.yaml",
        },

        // GoogleFCM IP
        GoogleFCM_ip: {
            ...ruleAnchor.classical,
            url: "https://raw.githubusercontent.com/RealSeek/Clash_Rule_DIY/refs/heads/mihomo/DIRECT/ip/GoogleFCM_ip.yaml",
            path: "./ruleset/RealSeek/Clash_Rule_DIY/DIRECT/ip/GoogleFCM_ip.yaml",
        },

        // 内网域名和局域网 IP
        Lan_ip: {
            ...ruleAnchor.classical,
            url: "https://raw.githubusercontent.com/RealSeek/Clash_Rule_DIY/refs/heads/mihomo/DIRECT/ip/Lan_ip.yaml",
            path: "./ruleset/RealSeek/Clash_Rule_DIY/DIRECT/ip/Lan_ip.yaml",
        },

        // 网易云音乐 ip
        NetEaseMusic_ip: {
            ...ruleAnchor.classical,
            url: "https://raw.githubusercontent.com/RealSeek/Clash_Rule_DIY/refs/heads/mihomo/DIRECT/ip/NetEaseMusic_ip.yaml",
            path: "./ruleset/RealSeek/Clash_Rule_DIY/DIRECT/ip/NetEaseMusic_ip.yaml",
        },

        // SteamCN IP
        SteamCN_ip: {
            ...ruleAnchor.classical,
            url: "https://raw.githubusercontent.com/RealSeek/Clash_Rule_DIY/refs/heads/mihomo/DIRECT/ip/SteamCN_ip.yaml",
            path: "./ruleset/RealSeek/Clash_Rule_DIY/DIRECT/ip/SteamCN_ip.yaml",
        },

        // ##################################################################################################################

        // apple CDN 云上贵州
        AppleCDN_no_ip: {
            ...ruleAnchor.domain,
            url: "https://raw.githubusercontent.com/RealSeek/Clash_Rule_DIY/refs/heads/mihomo/DIRECT/no_ip/AppleCDN_no_ip.yaml",
            path: "./ruleset/RealSeek/Clash_Rule_DIY/DIRECT/no_ip/AppleCDN_no_ip.yaml",
        },

        // 苹果直连域名
        AppleCN_no_ip: {
            ...ruleAnchor.domain,
            url: "https://raw.githubusercontent.com/RealSeek/Clash_Rule_DIY/refs/heads/mihomo/DIRECT/no_ip/AppleCN_no_ip.yaml",
            path: "./ruleset/RealSeek/Clash_Rule_DIY/DIRECT/no_ip/AppleCN_no_ip.yaml",
        },

        // 国内常见互联网公司和服务的域名
        Direct_no_ip: {
            ...ruleAnchor.classical,
            url: "https://raw.githubusercontent.com/RealSeek/Clash_Rule_DIY/refs/heads/mihomo/DIRECT/no_ip/Direct_no_ip.yaml",
            path: "./ruleset/RealSeek/Clash_Rule_DIY/DIRECT/no_ip/Direct_no_ip.yaml",
        },

        // 国内常见互联网公司和服务的域名
        Domestic_no_ip: {
            ...ruleAnchor.classical,
            url: "https://raw.githubusercontent.com/RealSeek/Clash_Rule_DIY/refs/heads/mihomo/DIRECT/no_ip/Domestic_no_ip.yaml",
            path: "./ruleset/RealSeek/Clash_Rule_DIY/DIRECT/no_ip/Domestic_no_ip.yaml",
        },

        // Google Fcm no ip
        GoogleFCM_no_ip: {
            ...ruleAnchor.classical,
            url: "https://raw.githubusercontent.com/RealSeek/Clash_Rule_DIY/refs/heads/mihomo/DIRECT/no_ip/GoogleFCM_no_ip.yaml",
            path: "./ruleset/RealSeek/Clash_Rule_DIY/DIRECT/no_ip/GoogleFCM_no_ip.yaml",
        },

        // 内网域名和局域网 IP
        Lan_no_ip: {
            ...ruleAnchor.classical,
            url: "https://raw.githubusercontent.com/RealSeek/Clash_Rule_DIY/refs/heads/mihomo/DIRECT/no_ip/Lan_no_ip.yaml",
            path: "./ruleset/RealSeek/Clash_Rule_DIY/DIRECT/no_ip/Lan_no_ip.yaml",
        },

        // 微软中国 CDN
        MicrosoftCDN_no_ip: {
            ...ruleAnchor.classical,
            url: "https://raw.githubusercontent.com/RealSeek/Clash_Rule_DIY/refs/heads/mihomo/DIRECT/no_ip/MicrosoftCDN_no_ip.yaml",
            path: "./ruleset/RealSeek/Clash_Rule_DIY/DIRECT/no_ip/MicrosoftCDN_no_ip.yaml",
        },

        // 网易云音乐域名
        NetEaseMusic_no_ip: {
            ...ruleAnchor.classical,
            url: "https://raw.githubusercontent.com/RealSeek/Clash_Rule_DIY/refs/heads/mihomo/DIRECT/no_ip/NetEaseMusic_no_ip.yaml",
            path: "./ruleset/RealSeek/Clash_Rule_DIY/DIRECT/no_ip/NetEaseMusic_no_ip.yaml",
        },

        // SteamCN 域名
        SteamCN_no_ip: {
            ...ruleAnchor.classical,
            url: "https://raw.githubusercontent.com/RealSeek/Clash_Rule_DIY/refs/heads/mihomo/DIRECT/no_ip/SteamCN_no_ip.yaml",
            path: "./ruleset/RealSeek/Clash_Rule_DIY/DIRECT/no_ip/SteamCN_no_ip.yaml",
        },

        // ##################################################################################################################

        /**
         * 代理部分
         */

        // ##################################################################################################################

        // 流媒体 IP
        Stream_ip: {
            ...ruleAnchor.classical,
            url: "https://raw.githubusercontent.com/RealSeek/Clash_Rule_DIY/refs/heads/mihomo/PROXY/ip/Stream_ip.yaml",
            path: "./ruleset/RealSeek/Clash_Rule_DIY/PROXY/ip/Stream_ip.yaml",
        },

        // telegram ip
        Telegram_ip: {
            ...ruleAnchor.classical,
            url: "https://raw.githubusercontent.com/RealSeek/Clash_Rule_DIY/refs/heads/mihomo/PROXY/ip/Telegram_ip.yaml",
            path: "./ruleset/RealSeek/Clash_Rule_DIY/PROXY/ip/Telegram_ip.yaml",
        },

        // ##################################################################################################################

        // ai 相关 包含 OpenAI、Google Gemini、Claude、Perplexity 等
        AI_no_ip: {
            ...ruleAnchor.classical,
            url: "https://raw.githubusercontent.com/RealSeek/Clash_Rule_DIY/refs/heads/mihomo/PROXY/no_ip/AI_no_ip.yaml",
            path: "./ruleset/RealSeek/Clash_Rule_DIY/PROXY/no_ip/AI_no_ip.yaml",
        },

        // apple
        Apple_no_ip: {
            ...ruleAnchor.classical,
            url: "https://raw.githubusercontent.com/RealSeek/Clash_Rule_DIY/refs/heads/mihomo/PROXY/no_ip/Apple_no_ip.yaml",
            path: "./ruleset/RealSeek/Clash_Rule_DIY/PROXY/no_ip/Apple_no_ip.yaml",
        },

        // 常需要代理的静态 CDN
        CDN_domainset: {
            ...ruleAnchor.domain,
            url: "https://raw.githubusercontent.com/RealSeek/Clash_Rule_DIY/refs/heads/mihomo/PROXY/no_ip/CDN_domainset.yaml",
            path: "./ruleset/RealSeek/Clash_Rule_DIY/PROXY/no_ip/CDN_domainset.yaml",
        },

        // 常需要代理的静态 CDN
        CDN_no_ip: {
            ...ruleAnchor.classical,
            url: "https://raw.githubusercontent.com/RealSeek/Clash_Rule_DIY/refs/heads/mihomo/PROXY/no_ip/CDN_no_ip.yaml",
            path: "./ruleset/RealSeek/Clash_Rule_DIY/PROXY/no_ip/CDN_no_ip.yaml",
        },

        // 存放着个人遇到需要代理的域名
        CustomProxy_no_ip: {
            ...ruleAnchor.classical,
            url: "https://raw.githubusercontent.com/RealSeek/Clash_Rule_DIY/refs/heads/mihomo/PROXY/no_ip/CustomProxy_no_ip.yaml",
            path: "./ruleset/RealSeek/Clash_Rule_DIY/PROXY/no_ip/CustomProxy_no_ip.yaml",
        },

        // 软件更新、操作系统等大文件下载
        Download_domainset: {
            ...ruleAnchor.domain,
            url: "https://raw.githubusercontent.com/RealSeek/Clash_Rule_DIY/refs/heads/mihomo/PROXY/no_ip/Download_domainset.yaml",
            path: "./ruleset/RealSeek/Clash_Rule_DIY/PROXY/no_ip/Download_domainset.yaml",
        },

        // 软件更新、操作系统等大文件下载
        Download_no_ip: {
            ...ruleAnchor.classical,
            url: "https://raw.githubusercontent.com/RealSeek/Clash_Rule_DIY/refs/heads/mihomo/PROXY/no_ip/Download_no_ip.yaml",
            path: "./ruleset/RealSeek/Clash_Rule_DIY/PROXY/no_ip/Download_no_ip.yaml",
        },

        // 常见海外服务和互联网公司的域名 有部分域名被DNS污染，故使用代理
        Global_no_ip: {
            ...ruleAnchor.classical,
            url: "https://raw.githubusercontent.com/RealSeek/Clash_Rule_DIY/refs/heads/mihomo/PROXY/no_ip/Global_no_ip.yaml",
            path: "./ruleset/RealSeek/Clash_Rule_DIY/PROXY/no_ip/Global_no_ip.yaml",
        },

        // 微软需要代理的域名
        Microsoft_no_ip: {
            ...ruleAnchor.classical,
            url: "https://raw.githubusercontent.com/RealSeek/Clash_Rule_DIY/refs/heads/mihomo/PROXY/no_ip/Microsoft_no_ip.yaml",
            path: "./ruleset/RealSeek/Clash_Rule_DIY/PROXY/no_ip/Microsoft_no_ip.yaml",
        },

        // Steam 需要代理的域名
        Steam_no_ip: {
            ...ruleAnchor.classical,
            url: "https://raw.githubusercontent.com/RealSeek/Clash_Rule_DIY/refs/heads/mihomo/PROXY/no_ip/Steam_no_ip.yaml",
            path: "./ruleset/RealSeek/Clash_Rule_DIY/PROXY/no_ip/Steam_no_ip.yaml",
        },

        // 流媒体域名
        Stream_no_ip: {
            ...ruleAnchor.classical,
            url: "https://raw.githubusercontent.com/RealSeek/Clash_Rule_DIY/refs/heads/mihomo/PROXY/no_ip/Stream_no_ip.yaml",
            path: "./ruleset/RealSeek/Clash_Rule_DIY/PROXY/no_ip/Stream_no_ip.yaml",
        },

        // telegram 域名
        Telegram_no_ip: {
            ...ruleAnchor.classical,
            url: "https://raw.githubusercontent.com/RealSeek/Clash_Rule_DIY/refs/heads/mihomo/PROXY/no_ip/Telegram_no_ip.yaml",
            path: "./ruleset/RealSeek/Clash_Rule_DIY/PROXY/no_ip/Telegram_no_ip.yaml",
        },

        // ##################################################################################################################
    };

    // 插入远程规则
    params["rule-providers"] = ruleProviders;
}

function getProxiesByRegexOne(params, regex) {
    return params.proxies.filter((e) => regex.test(e.name)).map((e) => e.name);
}

function getProxiesByRegex(params, regex) {
    const matchedProxies = params.proxies
        .filter((e) => regex.test(e.name))
        .map((e) => e.name);
    return matchedProxies.length > 0 ? matchedProxies : ["手动选择"];
}

// 修改DNS
function overwriteDns(params) {
    const dnsOptions = {
        enable: true,
        "enhanced-mode": "fake-ip", // fake-ip 或 redir-host
        "fake-ip-range": "198.18.0.1/16",
        "prefer-h3": true, // 如果 DNS 服务器支持 DoH3 会优先使用 h3
        "use-hosts": false,
        "use-system-hosts": false,
        ipv6: false,

        "fake-ip-filter": [
            "+.+m2m",
            "+.$injections.adguard.org",
            "+.$local.adguard.org",
            "+.+_tcp",
            "+.+bogon",
            "+.+_msdcs",
            "+.10.in-addr.arpa",
            "+.10.in-addr.arpa",
            "+.16.172.in-addr.arpa",
            "+.17.172.in-addr.arpa",
            "+.18.172.in-addr.arpa",
            "+.19.172.in-addr.arpa",
            "+.20.172.in-addr.arpa",
            "+.21.172.in-addr.arpa",
            "+.22.172.in-addr.arpa",
            "+.23.172.in-addr.arpa",
            "+.24.172.in-addr.arpa",
            "+.25.172.in-addr.arpa",
            "+.26.172.in-addr.arpa",
            "+.27.172.in-addr.arpa",
            "+.28.172.in-addr.arpa",
            "+.29.172.in-addr.arpa",
            "+.30.172.in-addr.arpa",
            "+.31.172.in-addr.arpa",
            "+.168.192.in-addr.arpa",
            "+.254.169.in-addr.arpa",
            "*.srv.nintendo.net",
            "*.stun.playstation.net",
            "*.turn.twilio.com",
            "*.stun.twilio.com",
            "stun.syncthing.net",
            "stun.*",

            // LAN
            "+.lan",
            "*.localdomain",
            "*.example",
            "*.invalid",
            "*.localhost",
            "*.test",
            "*.local",
            "*.home.arpa",

            // ntp
            "time.*.com",
            "time.*.gov",
            "time.*.edu.cn",
            "time.*.apple.com",
            "time-ios.apple.com",
            "time1.*.com",
            "time2.*.com",
            "time3.*.com",
            "time4.*.com",
            "time5.*.com",
            "time6.*.com",
            "time7.*.com",
            "ntp.*.com",
            "ntp1.*.com",
            "ntp2.*.com",
            "ntp3.*.com",
            "ntp4.*.com",
            "ntp5.*.com",
            "ntp6.*.com",
            "ntp7.*.com",
            "*.time.edu.cn",
            "*.ntp.org.cn",
            "+.pool.ntp.org",
            "time1.cloud.tencent.com",

            // 网易云音乐
            "music.163.com",
            "*.music.163.com",
            "*.126.net",

            // 百度音乐
            "musicapi.taihe.com",
            "music.taihe.com",

            // 酷狗音乐
            "songsearch.kugou.com",
            "trackercdn.kugou.com",

            // 酷我音乐
            "*.kuwo.cn",

            // JOOX音乐
            "api-jooxtt.sanook.com",
            "api.joox.com",
            "joox.com",

            // QQ音乐
            "y.qq.com",
            "*.y.qq.com",
            "streamoc.music.tc.qq.com",
            "mobileoc.music.tc.qq.com",
            "isure.stream.qqmusic.qq.com",
            "dl.stream.qqmusic.qq.com",
            "aqqmusic.tc.qq.com",
            "amobile.music.tc.qq.com",

            // 虾米音乐
            "*.xiami.com",

            // 咪咕音乐
            "*.music.migu.cn",
            "music.migu.cn",

            // windows 本地连接检测
            "+.msftconnecttest.com",
            "+.msftncsi.com",

            // QQ登录
            "localhost.ptlogin2.qq.com",
            "localhost.sec.qq.com",
            "+.qq.com",
            "+.tencent.com",

            // Steam
            "+.steamcontent.com",

            // Nintendo Switch
            "+.srv.nintendo.net",
            "*.n.n.srv.nintendo.net",
            "+.cdn.nintendo.net",

            // Sony PlayStation
            "+.stun.playstation.net",

            // Microsoft Xbox
            "xbox.*.*.microsoft.com",
            "*.*.xboxlive.com",
            "xbox.*.microsoft.com",
            "xnotify.xboxlive.com",

            // battlenet
            "+.battlenet.com.cn",

            // STUN
            "stun.*.*",
            "stun.*.*.*",
            "+.stun.*.*",
            "+.stun.*.*.*",
            "+.stun.*.*.*.*",
            "+.stun.*.*.*.*.*",

            // Netflix
            "+.nflxvideo.net",

            // Bilibili
            "*.mcdn.bilivideo.cn",

            // 米家
            "Mijia Cloud",

            // Xiaomi
            "+.market.xiaomi.com",

            // 招商银行
            "+.cmbchina.com",
            "+.cmbimg.com",

            // ADGuard
            "adguardteam.github.io",
            "adrules.top",
            "anti-ad.net",
            "local.adguard.org",
            "static.adtidy.org",

            // 迅雷
            "+.sandai.net",
            "+.n0808.com",

            // UU
            "+.uu.163.com",
            "ps.res.netease.com",

            // 向日葵远程控制
            "+.oray.com",
            "+.orayimg.com",

            "+.wggames.cn",

            //

            "WORKGROUP",
        ],

        // 默认的域名解析服务器
        nameserver: [
            "https://223.5.5.5/dns-query", // 阿里云
            "https://120.53.53.53/dns-query", // DNSPod
        ],

        // 代理节点域名解析服务器，仅用于解析代理节点的域名，如果不填则遵循nameserver-policy、nameserver和fallback的配置
        "proxy-server-nameserver": [
            "https://223.5.5.5/dns-query", // 阿里云
            "https://120.53.53.53/dns-query", // DNSPod
        ],

        // 指定域名查询的解析服务器，可使用 geosite, 优先于 nameserver/fallback 查询
        "nameserver-policy": {
            "dns.alidns.com": "quic://223.5.5.5:853",
            "doh.pub": "https://1.12.12.12/dns-query",
            "doh.360.cn": "101.198.198.198",
            "+.uc.cn": "quic://dns.alidns.com:853",
            "+.alibaba.com": "quic://dns.alidns.com:853",
            "*.alicdn.com": "quic://dns.alidns.com:853",
            "*.ialicdn.com": "quic://dns.alidns.com:853",
            "*.myalicdn.com": "quic://dns.alidns.com:853",
            "*.alidns.com": "quic://dns.alidns.com:853",
            "*.aliimg.com": "quic://dns.alidns.com:853",
            "+.aliyun.com": "quic://dns.alidns.com:853",
            "*.aliyuncs.com": "quic://dns.alidns.com:853",
            "*.alikunlun.com": "quic://dns.alidns.com:853",
            "*.alikunlun.net": "quic://dns.alidns.com:853",
            "*.cdngslb.com": "quic://dns.alidns.com:853",
            "+.alipay.com": "quic://dns.alidns.com:853",
            "+.alipay.cn": "quic://dns.alidns.com:853",
            "+.alipay.com.cn": "quic://dns.alidns.com:853",
            "*.alipayobjects.com": "quic://dns.alidns.com:853",
            "+.alibaba-inc.com": "quic://dns.alidns.com:853",
            "*.alibabausercontent.com": "quic://dns.alidns.com:853",
            "*.alibabadns.com": "quic://dns.alidns.com:853",
            "+.alicloudccp.com": "quic://dns.alidns.com:853",
            "+.alipan.com": "quic://dns.alidns.com:853",
            "+.aliyundrive.com": "quic://dns.alidns.com:853",
            "+.aliyundrive.net": "quic://dns.alidns.com:853",
            "+.cainiao.com": "quic://dns.alidns.com:853",
            "+.cainiao.com.cn": "quic://dns.alidns.com:853",
            "+.cainiaoyizhan.com": "quic://dns.alidns.com:853",
            "+.guoguo-app.com": "quic://dns.alidns.com:853",
            "+.etao.com": "quic://dns.alidns.com:853",
            "+.yitao.com": "quic://dns.alidns.com:853",
            "+.1688.com": "quic://dns.alidns.com:853",
            "+.amap.com": "quic://dns.alidns.com:853",
            "+.gaode.com": "quic://dns.alidns.com:853",
            "+.autonavi.com": "quic://dns.alidns.com:853",
            "+.dingtalk.com": "quic://dns.alidns.com:853",
            "+.mxhichina.com": "quic://dns.alidns.com:853",
            "+.soku.com": "quic://dns.alidns.com:853",
            "+.tb.cn": "quic://dns.alidns.com:853",
            "+.taobao.com": "quic://dns.alidns.com:853",
            "*.taobaocdn.com": "quic://dns.alidns.com:853",
            "*.tbcache.com": "quic://dns.alidns.com:853",
            "+.tmall.com": "quic://dns.alidns.com:853",
            "+.xiami.com": "quic://dns.alidns.com:853",
            "+.xiami.net": "quic://dns.alidns.com:853",
            "*.ykimg.com": "quic://dns.alidns.com:853",
            "+.youku.com": "quic://dns.alidns.com:853",
            "+.tudou.com": "quic://dns.alidns.com:853",
            "*.cibntv.net": "quic://dns.alidns.com:853",
            "+.ele.me": "quic://dns.alidns.com:853",
            "*.elemecdn.com": "quic://dns.alidns.com:853",
            "+.feizhu.com": "quic://dns.alidns.com:853",
            "+.taopiaopiao.com": "quic://dns.alidns.com:853",
            "+.fliggy.com": "quic://dns.alidns.com:853",
            "+.koubei.com": "quic://dns.alidns.com:853",
            "+.mybank.cn": "quic://dns.alidns.com:853",
            "+.mmstat.com": "quic://dns.alidns.com:853",
            "+.uczzd.cn": "quic://dns.alidns.com:853",
            "+.iconfont.cn": "quic://dns.alidns.com:853",
            "+.freshhema.com": "quic://dns.alidns.com:853",
            "+.hemamax.com": "quic://dns.alidns.com:853",
            "+.hemaos.com": "quic://dns.alidns.com:853",
            "+.hemashare.cn": "quic://dns.alidns.com:853",
            "+.shyhhema.com": "quic://dns.alidns.com:853",
            "+.sm.cn": "quic://dns.alidns.com:853",
            "+.npmmirror.com": "quic://dns.alidns.com:853",
            "+.alios.cn": "quic://dns.alidns.com:853",
            "+.wandoujia.com": "quic://dns.alidns.com:853",
            "+.aligames.com": "quic://dns.alidns.com:853",
            "+.25pp.com": "quic://dns.alidns.com:853",
            "*.aliapp.org": "quic://dns.alidns.com:853",
            "+.tanx.com": "quic://dns.alidns.com:853",
            "+.hellobike.com": "quic://dns.alidns.com:853",
            "*.hichina.com": "quic://dns.alidns.com:853",
            "*.yunos.com": "quic://dns.alidns.com:853",
            "*.qcloud.com": "https://doh.pub/dns-query",
            "*.gtimg.cn": "https://doh.pub/dns-query",
            "*.gtimg.com": "https://doh.pub/dns-query",
            "*.gtimg.com.cn": "https://doh.pub/dns-query",
            "*.gdtimg.com": "https://doh.pub/dns-query",
            "*.idqqimg.com": "https://doh.pub/dns-query",
            "*.udqqimg.com": "https://doh.pub/dns-query",
            "*.igamecj.com": "https://doh.pub/dns-query",
            "+.myapp.com": "https://doh.pub/dns-query",
            "*.myqcloud.com": "https://doh.pub/dns-query",
            "+.dnspod.com": "https://doh.pub/dns-query",
            "*.qpic.cn": "https://doh.pub/dns-query",
            "*.qlogo.cn": "https://doh.pub/dns-query",
            "+.qq.com": "https://doh.pub/dns-query",
            "+.qq.com.cn": "https://doh.pub/dns-query",
            "*.qqmail.com": "https://doh.pub/dns-query",
            "+.qzone.com": "https://doh.pub/dns-query",
            "*.tencent-cloud.net": "https://doh.pub/dns-query",
            "*.tencent-cloud.com": "https://doh.pub/dns-query",
            "+.tencent.com": "https://doh.pub/dns-query",
            "+.tencent.com.cn": "https://doh.pub/dns-query",
            "+.tencentmusic.com": "https://doh.pub/dns-query",
            "+.weixinbridge.com": "https://doh.pub/dns-query",
            "+.weixin.com": "https://doh.pub/dns-query",
            "+.weiyun.com": "https://doh.pub/dns-query",
            "+.soso.com": "https://doh.pub/dns-query",
            "+.sogo.com": "https://doh.pub/dns-query",
            "+.sogou.com": "https://doh.pub/dns-query",
            "*.sogoucdn.com": "https://doh.pub/dns-query",
            "*.roblox.cn": "https://doh.pub/dns-query",
            "+.robloxdev.cn": "https://doh.pub/dns-query",
            "+.wegame.com": "https://doh.pub/dns-query",
            "+.wegame.com.cn": "https://doh.pub/dns-query",
            "+.wegameplus.com": "https://doh.pub/dns-query",
            "+.cdn-go.cn": "https://doh.pub/dns-query",
            "*.tencentcs.cn": "https://doh.pub/dns-query",
            "*.qcloudimg.com": "https://doh.pub/dns-query",
            "+.dnspod.cn": "https://doh.pub/dns-query",
            "+.anticheatexpert.com": "https://doh.pub/dns-query",
            "url.cn": "https://doh.pub/dns-query",
            "*.qlivecdn.com": "https://doh.pub/dns-query",
            "*.tcdnlive.com": "https://doh.pub/dns-query",
            "*.dnsv1.com": "https://doh.pub/dns-query",
            "upos-sz-mirrorali.bilivideo.com": "quic://dns.alidns.com:853",
            "upos-sz-estgoss.bilivideo.com": "quic://dns.alidns.com:853",
            "upos-sz-mirrorbd.bilivideo.com": "180.76.76.76",
            "upos-sz-mirrorbos.bilivideo.com": "180.76.76.76",
            "upos-sz-mirrorcosbstar1.bilivideo.com": "https://doh.pub/dns-query",
            "acg.tv": "https://doh.pub/dns-query",
            "b23.tv": "https://doh.pub/dns-query",
            "+.bilibili.cn": "https://doh.pub/dns-query",
            "+.bilibili.com": "https://doh.pub/dns-query",
            "*.acgvideo.com": "https://doh.pub/dns-query",
            "*.bilivideo.com": "https://doh.pub/dns-query",
            "*.bilivideo.cn": "https://doh.pub/dns-query",
            "*.bilivideo.net": "https://doh.pub/dns-query",
            "*.hdslb.com": "https://doh.pub/dns-query",
            "*.biliimg.com": "https://doh.pub/dns-query",
            "*.biliapi.com": "https://doh.pub/dns-query",
            "*.biliapi.net": "https://doh.pub/dns-query",
            "+.biligame.com": "https://doh.pub/dns-query",
            "*.biligame.net": "https://doh.pub/dns-query",
            "+.bilicomic.com": "https://doh.pub/dns-query",
            "+.bilicomics.com": "https://doh.pub/dns-query",
            "*.bilicdn1.com": "https://doh.pub/dns-query",
            "+.mi.com": "https://doh.pub/dns-query",
            "+.duokan.com": "https://doh.pub/dns-query",
            "*.mi-img.com": "https://doh.pub/dns-query",
            "*.mi-idc.com": "https://doh.pub/dns-query",
            "*.xiaoaisound.com": "https://doh.pub/dns-query",
            "*.xiaomixiaoai.com": "https://doh.pub/dns-query",
            "*.mi-fds.com": "https://doh.pub/dns-query",
            "*.mifile.cn": "https://doh.pub/dns-query",
            "*.mijia.tech": "https://doh.pub/dns-query",
            "+.miui.com": "https://doh.pub/dns-query",
            "+.xiaomi.com": "https://doh.pub/dns-query",
            "+.xiaomi.cn": "https://doh.pub/dns-query",
            "+.xiaomi.net": "https://doh.pub/dns-query",
            "+.xiaomiev.com": "https://doh.pub/dns-query",
            "+.xiaomiyoupin.com": "https://doh.pub/dns-query",
            "+.bytedance.com.com": "180.184.2.2",
            "*.bytecdn.cn": "180.184.2.2",
            "*.volccdn.com": "180.184.2.2",
            "*.toutiaoimg.com": "180.184.2.2",
            "*.toutiaoimg.cn": "180.184.2.2",
            "*.toutiaostatic.com": "180.184.2.2",
            "*.toutiaovod.com": "180.184.2.2",
            "*.toutiaocloud.com": "180.184.2.2",
            "+.toutiaopage.com": "180.184.2.2",
            "+.feiliao.com": "180.184.2.2",
            "+.iesdouyin.com": "180.184.2.2",
            "*.pstatp.com": "180.184.2.2",
            "+.snssdk.com": "180.184.2.2",
            "*.bytegoofy.com": "180.184.2.2",
            "+.toutiao.com": "180.184.2.2",
            "+.feishu.cn": "180.184.2.2",
            "+.feishu.net": "180.184.2.2",
            "*.feishucdn.com": "180.184.2.2",
            "*.feishupkg.com": "180.184.2.2",
            "+.douyin.com": "180.184.2.2",
            "*.douyinpic.com": "180.184.2.2",
            "*.douyinstatic.com": "180.184.2.2",
            "*.douyincdn.com": "180.184.2.2",
            "*.douyinliving.com": "180.184.2.2",
            "*.douyinvod.com": "180.184.2.2",
            "+.huoshan.com": "180.184.2.2",
            "*.huoshanstatic.com": "180.184.2.2",
            "+.huoshanzhibo.com": "180.184.2.2",
            "+.ixigua.com": "180.184.2.2",
            "*.ixiguavideo.com": "180.184.2.2",
            "*.ixgvideo.com": "180.184.2.2",
            "*.byted-static.com": "180.184.2.2",
            "+.volces.com": "180.184.2.2",
            "+.baike.com": "180.184.2.2",
            "*.zjcdn.com": "180.184.2.2",
            "*.zijieapi.com": "180.184.2.2",
            "+.feelgood.cn": "180.184.2.2",
            "*.bytetcc.com": "180.184.2.2",
            "*.bytednsdoc.com": "180.184.2.2",
            "*.byteimg.com": "180.184.2.2",
            "*.byteacctimg.com": "180.184.2.2",
            "*.ibytedapm.com": "180.184.2.2",
            "+.oceanengine.com": "180.184.2.2",
            "+.91.com": "180.76.76.76",
            "+.hao123.com": "180.76.76.76",
            "+.baidu.cn": "180.76.76.76",
            "+.baidu.com": "180.76.76.76",
            "+.iqiyi.com": "180.76.76.76",
            "*.iqiyipic.com": "180.76.76.76",
            "*.baidubce.com": "180.76.76.76",
            "*.bcelive.com": "180.76.76.76",
            "*.baiducontent.com": "180.76.76.76",
            "*.baidustatic.com": "180.76.76.76",
            "*.bdstatic.com": "180.76.76.76",
            "*.bdimg.com": "180.76.76.76",
            "*.bcebos.com": "180.76.76.76",
            "*.baidupcs.com": "180.76.76.76",
            "*.baidubcr.com": "180.76.76.76",
            "*.yunjiasu-cdn.net": "180.76.76.76",
            "+.tieba.com": "180.76.76.76",
            "+.xiaodutv.com": "180.76.76.76",
            "*.shifen.com": "180.76.76.76",
            "*.jomodns.com": "180.76.76.76",
            "*.bdydns.com": "180.76.76.76",
            "*.jomoxc.com": "180.76.76.76",
            "*.duapp.com": "180.76.76.76",
            "*.antpcdn.com": "180.76.76.76",
            "*.qhimg.com": "https://doh.360.cn/dns-query",
            "*.qhimgs.com": "https://doh.360.cn/dns-query",
            "*.qhimgs?.com": "https://doh.360.cn/dns-query",
            "*.qhres.com": "https://doh.360.cn/dns-query",
            "*.qhres2.com": "https://doh.360.cn/dns-query",
            "*.qhmsg.com": "https://doh.360.cn/dns-query",
            "*.qhstatic.com": "https://doh.360.cn/dns-query",
            "*.qhupdate.com": "https://doh.360.cn/dns-query",
            "*.qihucdn.com": "https://doh.360.cn/dns-query",
            "+.360.com": "https://doh.360.cn/dns-query",
            "+.360.cn": "https://doh.360.cn/dns-query",
            "+.360.net": "https://doh.360.cn/dns-query",
            "+.360safe.com": "https://doh.360.cn/dns-query",
            "*.360tpcdn.com": "https://doh.360.cn/dns-query",
            "+.360os.com": "https://doh.360.cn/dns-query",
            "*.360webcache.com": "https://doh.360.cn/dns-query",
            "+.360kuai.com": "https://doh.360.cn/dns-query",
            "+.so.com": "https://doh.360.cn/dns-query",
            "+.haosou.com": "https://doh.360.cn/dns-query",
            "+.yunpan.cn": "https://doh.360.cn/dns-query",
            "+.yunpan.com": "https://doh.360.cn/dns-query",
            "+.yunpan.com.cn": "https://doh.360.cn/dns-query",
            "*.qh-cdn.com": "https://doh.360.cn/dns-query",
            "+.baomitu.com": "https://doh.360.cn/dns-query",
            "+.qiku.com": "https://doh.360.cn/dns-query",
            "+.securelogin.com.cn": ["system://", "system", "dhcp://system"],
            "captive.apple.com": ["system://", "system", "dhcp://system"],
            "hotspot.cslwifi.com": ["system://", "system", "dhcp://system"],
            "*.m2m": ["system://", "system", "dhcp://system"],
            "injections.adguard.org": ["system://", "system", "dhcp://system"],
            "local.adguard.org": ["system://", "system", "dhcp://system"],
            "*._tcp": ["system://", "system", "dhcp://system"],
            "*.bogon": ["system://", "system", "dhcp://system"],
            "*._msdcs": ["system://", "system", "dhcp://system"],
        },
    };

    params["dns"] = dnsOptions;
}

function getManualProxiesByRegex(params, regex) {
    const matchedProxies = params.proxies
        .filter((e) => regex.test(e.name))
        .map((e) => e.name);
    return matchedProxies.length > 0
        ? matchedProxies
        : ["DIRECT", "手动选择", proxyName];
}

// 覆写Tunnel
function overwriteTunnel (params) {
    const tunnelOptions = {
        enable: true,
        stack: "system",
        device: "Mihomo",
        "dns-hijack": ["any:53", "tcp://any:53"],
        "auto-route": true,
        "auto-detect-interface": true,
        "strict-route": true,
        "route-exclude-address": ["10.100.10.1/24"],
    };
    params.tun = { ...tunnelOptions };
}
```
