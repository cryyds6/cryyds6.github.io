浏览器打开B站关注页面，打开浏览器控制台，输入以下脚本即可。如果有报错或繁忙刷新重新开始就行。
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