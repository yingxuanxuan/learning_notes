# gitbook

### gitbook space与github同步

* 打开gitbook右上角菜单栏，配置当前space
* 点击`Synchronize with Git`
* `Provider`选择`Github`，点击`Configure`
* `Authenticate`点击`Connect with GitHub`（弹出窗口连接账户）
* 下拉菜单选择`Account`
* 下拉菜单选择`Repository`（仓库需要提前在GitHub中创建）
* 下拉菜单选择`Branch`（新建空Repository首次提交需要手动输入默认branch名称）
* 选择`Project directory`（需要手动输入，留空为仓库根目录）
* 选择首次连接时同步方向（空`GitHub Repository`选择`GitBook to GitHub`）

<figure><img src=".gitbook/assets/1690254284615.png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/1690254313000.png" alt=""><figcaption></figcaption></figure>

### gitbook第一个page问题

* gitbook中创建的第一个page在git目录中保存为README.md
* 自动保存的README.md会阻碍在git目录中创建group和subpage目录
* 打开outline
