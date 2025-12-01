浏览器打开B站关注页面，打开浏览器控制台，输入以下脚本即可。如果有报错或繁忙刷新重新开始就行。

### 批量取消关注
#### 版本一，自动翻页
```
(async () => {
  // 等待函数
  const sleep = (s) => new Promise(resolve => setTimeout(resolve, s * 1000));
  
  // 取消关注当前页的所有用户
  const unfollowAll = async () => {
    let items = document.querySelectorAll('.follow-btn__trigger.gray');
    console.log('本页待取消关注数量:', items.length);
    
    for (let i = 0; i < items.length; i++) {
      console.log(`取消第 ${i+1}/${items.length} 个`);
      
      // 创建真实鼠标事件模拟点击
      const event = new MouseEvent('click', {
        view: window,
        bubbles: true,
        cancelable: true
      });  // 修正：添加了右括号
      items[i].dispatchEvent(event);
      
      await sleep(1.5); // 增加等待时间降低风控风险
    }
    
    return items.length;
  };
  
  // 查找并点击下一页按钮
  const clickNextPage = async () => {
    console.log('正在寻找下一页按钮...');
    
    // 针对B站实际页面结构优化的选择器
    const nextPageSelectors = [
      '.be-pager-next', // B站分页组件的下一页按钮
      '.be-pager-item:last-child a', // 分页区域的最后一个按钮
      'a[title="下一页"]', // 带title属性的按钮
      'button:contains("下一页")' // 包含"下一页"文本的按钮
    ];
    
    let nextButton = null;
    
    // 尝试多种选择器
    for (const selector of nextPageSelectors) {
      try {
        if (selector.includes(':contains')) {
          // 文本内容匹配
          const elements = document.querySelectorAll(selector.split(':contains')[0]);
          for (const el of elements) {
            if (el.textContent.includes('下一页')) {
              nextButton = el;
              break;
            }
          }
        } else {
          // 常规选择器
          nextButton = document.querySelector(selector);
        }
        
        if (nextButton && nextButton.offsetParent !== null) {
          console.log(`找到下一页按钮: ${selector}`);
          break;
        }
      } catch (e) {
        console.warn(`选择器 ${selector} 执行失败:`, e);
      }
    }
    
    if (nextButton) {
      // 检查按钮是否被禁用
      if (nextButton.classList.contains('disabled') || 
          nextButton.getAttribute('disabled') === 'disabled') {
        console.log('已到达最后一页，任务完成！');
        return false;
      }
      
      console.log('点击下一页按钮');
      
      // 创建真实鼠标事件模拟点击
      const event = new MouseEvent('click', {
        view: window,
        bubbles: true,
        cancelable: true
      });  // 修正：添加了右括号
      nextButton.dispatchEvent(event);
      
      await sleep(3); // 等待新页面加载
      return true;
    } else {
      console.log('未找到下一页按钮，尝试备用方案...');
      
      // 备用方案：直接获取分页按钮列表
      const paginationItems = document.querySelectorAll('.be-pager-item');
      if (paginationItems.length > 0) {
        const lastItem = paginationItems[paginationItems.length - 1];
        if (lastItem.textContent.includes('下一页')) {
          console.log('通过分页列表找到下一页按钮');
          
          const event = new MouseEvent('click', {
            view: window,
            bubbles: true,
            cancelable: true
          });  // 修正：添加了右括号
          lastItem.dispatchEvent(event);
          
          await sleep(3); // 等待新页面加载
          return true;
        }
      }
      
      console.log('未找到下一页按钮，任务可能已完成或需要手动检查');
      return false;
    }
  };
  
  // 主执行流程
  try {
    let pageCount = 1;
    
    while (true) {
      console.log(`\n===== 开始处理第 ${pageCount} 页 =====`);
      
      const processedCount = await unfollowAll();
      
      if (processedCount > 0) {
        console.log(`第 ${pageCount} 页已清理完毕！`);
      } else {
        console.log(`第 ${pageCount} 页没有需要取消关注的用户`);
      }
      
      // 等待页面稳定
      await sleep(2);
      
      // 尝试翻页
      const hasNext = await clickNextPage();
      
      if (!hasNext) {
        console.log('没有更多页面，任务完成！');
        break;
      }
      
      // 等待新页面内容加载
      console.log('等待新页面加载...');
      await sleep(4);
      
      pageCount++;
    }
  } catch (error) {
    console.error('执行过程中出错:', error);
    console.log('建议刷新页面后重新执行脚本');
  }
})();
```

#### 版本二
```
(async () => {
  const sleep = (ms) => new Promise(resolve => setTimeout(resolve, ms * 1000));
  let data = document.querySelectorAll('.follow-btn__trigger.gray');
  console.log('获取本页关注数量:', data.length);
  let dataIndex = 0;
  let pageIndex = 0;
  while (dataIndex < data.length) {
    const x = data[dataIndex];
    console.log(`正在取消第:${dataIndex + 1} 个关注`);
    x.click();
    await sleep(1);
    if (data.length - 1 === dataIndex) {
      let pages = document.querySelectorAll('.vui_button.vui_button--no-transition.vui_pagenation--btn.vui_pagenation--btn-num');
      await sleep(0.5);
      if (pages.length > 1) {
        pageIndex = pageIndex === 1 ? 0 : 1;
      } else {
        console.log(`没有更多的页面了`);
        break;
      }
      pages[pageIndex].click();
      await sleep(1);
      data = document.querySelectorAll('.follow-btn__trigger.gray');
      console.log('重新获取本页数量:', data.length);
      dataIndex = 0;
    } else {
      dataIndex++;
    }
  }
  console.log('已取消全部关注');
})();
```
### 批量移除粉丝
```
(async () => {
    const sleep = (s) => new Promise(resolve => setTimeout(resolve, s * 1000));
    for (var i = 0; i < 100; i++) {
        try {
            var parent = document.getElementsByClassName('vui_icon sic-BDC-more_vertical_fill icon')[0];
            var enterEvent = new MouseEvent('mouseenter', {
                bubbles: true,   // 允许事件冒泡
                cancelable: true, // 事件可被取消
                view: window      // 关联当前窗口
            });
            parent.dispatchEvent(enterEvent);
            var menuItems = document.getElementsByClassName('menu-popover__panel-item');
            for (var j = 0; j < menuItems.length; j++) {
                if (menuItems[j].innerText === '移除粉丝') {
                    console.log("成功移除粉丝");
                    menuItems[j].click();
                }
            }
            // 模拟鼠标离开（可选）
            const leaveEvent = new MouseEvent('mouseleave', { bubbles: true, cancelable: true });
            parent.dispatchEvent(leaveEvent);
            document.getElementsByClassName('vui_checkbox--label')[0].click();
            document.getElementsByClassName('vui_button vui_button--blue vui_dialog--btn vui_dialog--btn-confirm')[0].click();
        } catch (error) {
        }
        await sleep(1);   // 等待1秒防触发风控
    }
    console.log('清理完毕！重新执行脚本');
})();
```


如果是服务器挂可以用这个持续窗口bat
```
@echo off
setlocal enabledelayedexpansion

rem 查询 RDP 会话获取目标会话的 ID
for /f "tokens=3" %%a in ('query session ^| findstr /i "rdp-tcp#"') do (
    set session_id=%%a
)

rem 断开 RDP 会话并将连接重定向到控制台
tscon %session_id% /dest:console

endlocal
```
