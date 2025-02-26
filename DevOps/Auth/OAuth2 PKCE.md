---
title: OAuth2 PKCE
created: 2025-02-26 13:51
---


<!-- markdownlint-disable MD025 -->

# OAuth2 PKCE

OAuth 2.0 PKCE（Proof Key for Code Exchange，发音为 "pixy"）中的 Code Challenge 是用于增强 OAuth 授权码流程（Authorization Code Flow）安全性的一种机制，主要目的是防止 授权码拦截攻击（Authorization Code Interception Attack）。以下是其核心用途和原理：

1. **防止授权码被劫持**
   - **场景**：在传统 OAuth 授权码流程中，攻击者可能通过中间人攻击或恶意代理截获授权码（Authorization Code），然后用它换取访问令牌（Access Token）。
   - **PKCE 的作用**：PKCE 要求客户端生成一个临时的 Code Verifier（随机字符串）和对应的 Code Challenge（通常是 Code Verifier 的哈希值）。客户端在换取令牌时必须提供原始的 Code Verifier，服务器会验证其与 Code Challenge 是否匹配。即使攻击者截获了授权码，也无法通过验证（缺少 Code Verifier），从而无法获取令牌。
2. **保护公共客户端（Public Clients）**
   - **适用对象**：移动应用（Mobile Apps）、单页应用（SPA）、桌面应用等无法安全存储客户端密钥（Client Secret）的公共客户端。
   - **传统问题**：这些客户端无法使用客户端认证（Client Secret），导致授权码流程存在安全漏洞。
   - **PKCE 的解决方案**：通过 Code Challenge 替代客户端密钥，无需依赖 Client Secret，从而为公共客户端提供安全保障。
3. **Code Challenge 的生成和验证流程**
   - 步骤 1：客户端生成一个高熵的随机字符串 Code Verifier（如 32 字节的 Base64URL 编码字符串）。
   - 步骤 2：客户端对 Code Verifier 进行哈希（如 SHA-256）生成 Code Challenge，并将其附加到授权请求中。
   - 步骤 3：授权服务器存储 Code Challenge 和授权码的关联。
   - 步骤 4：当客户端用授权码换取令牌时，必须提供原始的 Code Verifier。服务器验证 Code Verifier 的哈希是否与 Code Challenge 一致，确保请求的合法性。
4. **标准化与强制使用**
   - **OAuth 2.1 规范**：PKCE 已成为 OAuth 2.1 的强制要求，所有公共客户端必须使用。
   - **兼容性**：即使服务端未强制要求，客户端主动使用 PKCE 也能提升安全性。

## Refer

- [RFC 7636: Proof Key for Code Exchange](https://www.rfc-editor.org/rfc/rfc7636)
- [OAuth pkce](https://oauth.net/2/pkce/)
- [PKCE generate tool](https://developer.pingidentity.com/en/tools/pkce-code-generator.html)
