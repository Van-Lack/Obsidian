

**DCA** – for _Dependency Confusion Attack_


⚡ Dependency Confusion via JS Miner

@GodfatherOrwa just landed a clean P1 by leveraging JS Miner in Burp Suite 🔥

Here’s how it went down 👇

⚡ After crawling all endpoints, he went to:
Target ➡ Extensions ➡ JS Miner ➡ Run All Passive Scans

⚡ That’s when he spotted: [JS Miner] Dependency Confusion
The vulnerable package was unclaimed on NPM 👀

⚡ Next steps he followed:

```bash
npm login
mkdir internal-tools && cd internal-tools
npm init -y
npm publish --access public
```

After claiming the package, he injected an RCE payload via package.json
Full POC: github.com/orwagodfather/NPM-RCE

**⚡ Result? A solid P1 vulnerability and a perfect example of how effective Dependency Confusion still is.**
