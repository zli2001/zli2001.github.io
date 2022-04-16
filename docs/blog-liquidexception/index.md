# Blog LiquidException

Solving the deploying problem.
<!--more-->
## Error
While I push my hugo project to github, action deploying went wrong: 

![Liquid](Liquid.png)


## Code

![Original.png](Original.png)

## Solve the problem
1. delete these two rows

   ![Solution1.png](Solution1.png)


2. change {{ to { { (add a space between them):

   (https://www.ucloud.cn/yun/39853.html):
![Solution2.png](Solution2.png)
## Trouble Shooting
There is problem description on the [jekyll](https://jekyllrb.com/docs/troubleshooting/#configuration-problems) pages but not solution.

And [add a.nojekyll](https://gitee.com/help/articles/4136#article-header1) doesn't work for my case.
