- [基于2048修改开源项目](https://github.com/gabrielecirulli/2048)

---

要修改的地方：

- 在`js`目录下的`game_manager.js` 修改`var value = Math.random() < 0.9 ? 2 : 4;`为`var value = Math.random() < 0.9 ? NameArray[0] : NameArray[1];`
- 在这个`js`的第一行，加了一句：`NameArray` =['夏','商','西周','春秋','战国','秦','汉','三国','两晋','南北朝','隋','唐','五代十国','宋','元','明','清','民国','新中国'];
- 在`game_manager.js`我们发现了如下一段代码：
```javascript
var merged = new Tile(positions.next, tile.value * 2);
merged.mergedFrom = [tile, next];

self.grid.insertTile(merged);
self.grid.removeTile(tile);

// Converge the two tiles' positions
tile.updatePosition(positions.next);

// Update the score
self.score += merged.value;

// The mighty 2048 tile
if (merged.value === 2048) self.won = true;
```
`tile.value`的值现在是一个字符串，所以不能简单的乘以2，我们可以先找到它在`NameArray`里的位置，然后取他的下一个值。

```javascript
var pos =NameArray.indexOf(tile.value);
var merged = new Tile(positions.next, NameArray[pos+1]);
```
另外，`merged.value`也不能直接加到`self.score`里去了，要改成:

```javascript
now_value=Math.pow(2,pos+2);
self.score += now_value;
if (now_value === 2048) self.won = true;
```

- 在`html_actuator.js`中，我们找到了这样一句： `var classes = ["tile", "tile-" + tile.value, positionClass];`
我们可以在这一行的前面，补上两句：

```javascript
var pos =NameArray.indexOf(tile.value);
var tile_value=Math.pow(2,pos+1);
//再修改一下刚才的那句：
var classes = ["tile", "tile-" + tile_value, positionClass];
```
- 至于字体大小的问题，我们得回到`main.css`中去找答案。

```css
.tile .tile-inner {
  border-radius: 3px;
  background: #eee4da;
  text-align: center;
  font-weight: bold;
  z-index: 10;
  font-size: 55px; }
```

`font-size: 55px`;，改成`font-size: 35px;`。

**最终的效果**

![](https://teamhost.gitbooks.io/learn-coding-with-open-source/content/zh/images/2048-4.png)
  
