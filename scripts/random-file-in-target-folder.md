## 写在前面
我将针对不同的场景提出不同的脚本，场景随缘更新，欢迎 Issue or PR

## 场景一
你需要在你的HomePage里单纯放一个按钮可以跳转到目标文件夹的随机文件，且每次点开不重复

```dataviewjs
const folder = "Interview Review";
let files = dv.pages(`"${folder}"`).file;

let readFiles = [];

const getUnreadFiles = () => {
	return files.filter(file => !readFiles.includes(file.path));
};

const openRandomUnreadFile = () => {
	const unreadFiles = getUnreadFiles();

	if (unreadFiles.length === 0) {
		dv.paragraph("Congratulations, all done!");
		return;
	}

	let randomFile = unreadFiles[Math.floor(Math.random() * unreadFiles.length)];
	readFiles.push(randomFile.path);

	app.workspace.openLinkText(randomFile.path, '/', true);

	updateStatus();
};

const updateStatus = () => {
	const unreadFiles = getUnreadFiles();
	const statusText = `已读: ${ readFiles.length }/${files.length} | 剩余: ${unreadFiles.length}`;

const oldStatus = dv.container.querySelector('.status-info');
if (oldStatus) oldStatus.remove();

dv.el('div', statusText, { cls: 'status-info' });
};

if (files.length > 0) {
	let btn = dv.el("button", "pick one!");
	btn.onclick = openRandomUnreadFile;

	let resetBtn = dv.el("button", "reset status!");
	resetBtn.onclick = () => {
		readFiles = [];
		updateStatus();
	};

	updateStatus();

	dv.el('style', `
        .status-info {
            margin: 10px 0;
            padding: 8px;
            background: var(--background-secondary);
            border-radius: 4px;
            font-size: 0.9em;
        }
        button {
            margin-right: 10px;
        }
    `);
} else {
	dv.paragraph("No Files in your folder!");
}
```

## 场景二

复习场景，你希望一篇文章之后接一个按钮可以跳转到另一个随机复习文章，暂时未作全局的记忆功能，所以你可能跳转到看过的页面（

```dataviewjs
const folder = "Interview Review";  
let files = dv.pages(`"${folder}"`).file;
if (files.length > 0) {
    let randomFile = files[Math.floor(Math.random() * files.length)];
    let btn = dv.el("button", "Pick One!");
    btn.onclick = () => {
        app.workspace.openLinkText(randomFile.path, '/', true);
    };
} else {
    dv.paragraph("No Files in your folder!");
```