Html解析
```Java
public void parse(String url){
  Parser parser = new Parser(url);
  NodeList list = parser.parse(null);
  if (list == null || list.toNodeArray().length == 0) {
    return;
  }
  parseNodeList(list.toNodeArray());
}
```Java

Css解析
```Java
InputSource source = new InputSource(new StringReader(cssContent));
CSSOMParser parser = new CSSOMParser(new SACParserCSS3());
CSSStyleSheet sheet = parser.parseStyleSheet(source, null, null);
CSSRuleList rules = sheet.getCssRules();
```Java
