Html解析
```java
import org.htmlparser.Node;
import org.htmlparser.Parser;
import org.htmlparser.nodes.TextNode;
import org.htmlparser.util.NodeList;
import org.htmlparser.util.ParserException;


public void parse(String url){
  Parser parser = new Parser(url);
  NodeList list = parser.parse(null);
  if (list == null || list.toNodeArray().length == 0) {
    return;
  }
  parseNodeList(list.toNodeArray());
}
```

Css解析
```Java
import com.steadystate.css.parser.CSSOMParser;
import com.steadystate.css.parser.SACParserCSS3;
import org.w3c.css.sac.InputSource;
import org.w3c.dom.css.CSSRule;
import org.w3c.dom.css.CSSRuleList;
import org.w3c.dom.css.CSSStyleSheet;


InputSource source = new InputSource(new StringReader(cssContent));
CSSOMParser parser = new CSSOMParser(new SACParserCSS3());
CSSStyleSheet sheet = parser.parseStyleSheet(source, null, null);
CSSRuleList rules = sheet.getCssRules();
```
