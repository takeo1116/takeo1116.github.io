<head>
  <script src="https://unpkg.com/mermaid@8.0.0/dist/mermaid.min.js"></script>
  <script>
  const mermaiding = function() {
      const elements = document.querySelectorAll("pre>code.language-mermaid");
      for (let i = 0; i < elements.length; i++) {
          const e = elements[i];
          const pre = e.parentElement;
          const replace = function(graph) {
              const elem = document.createElement('div');
              elem.innerHTML = graph;
              elem.className = 'mermaid';
              elem.setAttribute('data-processed', 'true');
              pre.parentElement.replaceChild(elem, pre);
          }
          mermaid.mermaidAPI.render('id' + i, e.textContent, replace);
      }
  }

  if (document.readyState == 'interactive' || document.readyState == 'complete') {
      mermaiding();
  }else{
      document.addEventListener("DOMContentLoaded", mermaiding);
  }
  </script>
</head>

竹雄（[takeo1116](https://twitter.com/takeo1116)）のホームページです。

現在、[旧ページ](https://takeo1116.sakura.ne.jp/)からコンテンツを移行中

## コンテンツ

- 競技プログラミング関係（移行中）
- [量子情報関係](./quantum/index.md)
- [機械学習関係](./machinelearning/index.md)

## テスト

```mermaid
graph LR
  A --> B --> C
```