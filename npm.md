

## npm 查看 pakcage 最新版本

```bash
npm dist-tag ls <package>
```
或

```bash
npm view <package> version
```


## npm 查看 pakcage 所有版本

```bash
npm view <package> versions --json
```

## npm 发布指定tag

```bash
npm public --tag next
```

## npm 把指定 tag 发布到 latest

```bash
npm dist-tag add <package>@<version> latest
```

```bash
npm dist-tag add easywebpack@4.0.0 latest
```

## npm 清除缓存

```bash
npm cache clean --force
```

## npm 指定registry

```bash
npm install --registry https://registry.npm.taobao.org
```