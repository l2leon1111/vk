Mass removal of posts from the VK wall | Массовое удаление постов с стены ВК
----------------------------------------------------------------------------------
🧹 VK Wall Post Remover (2025)
Automatically delete all posts from your VK wall

📌 Description
This JavaScript script automatically removes all posts from your personal wall on VK.com. It simulates user clicks to open the post menu, selects the “Delete” option, and confirms the deletion.

💡 Perfect for mass-cleaning your wall from old posts.

⚠️ Works with VK’s 2025 interface. The script uses up-to-date DOM selectors and class names.

🛠️ How to Use
Go to your VK page: https://vk.com/

Scroll down a bit to load a few posts

Open Developer Tools (F12)

Navigate to the Console tab

Paste the entire script and press Enter

Confirm the prompt when asked

The script will start scrolling and deleting all posts automatically.

📋 What the Script Does
Asks for user confirmation before starting

Finds and clicks the three dots menu (...) on each post

Selects the “Delete” option from the menu

Confirms the deletion

Continues until no more deletable posts are found

⏱️ Timing Configuration
You can tweak two delay settings in the script:

js
Копировать
Редактировать
const DELAY = 300;        // Delay between clicks (ms)
const SCROLL_DELAY = 800; // Delay after scrolling (ms)
Increase these values if your internet is slow or the site is lagging.

✅ Compatibility
✔️ Verified as of June 2025

✔️ Tested in Google Chrome

❌ Works only on your personal wall, not on other users' pages

⚠️ Warning
Deleted posts cannot be recovered. Use this tool at your own risk.

📄 License
MIT — free to use with no warranty.
-------------------------------------------------------------------
🧹 VK Wall Post Remover (2025)
Автоматическое удаление всех постов со стены ВКонтакте

📌 Описание
Этот JavaScript-скрипт удаляет все публикации на вашей личной стене во ВКонтакте автоматически. Он кликает по кнопке меню каждого поста, выбирает пункт «Удалить» и подтверждает удаление.

💡 Подходит для массовой очистки стены от старых записей.

⚠️ Работает с новым интерфейсом VK на 2025 год. Скрипт использует актуальные классы и структуру DOM.

🛠️ Как использовать
Откройте свою страницу ВКонтакте: https://vk.com/

Прокрутите немного вниз (загрузите несколько постов)

Откройте инструменты разработчика (F12)

Перейдите на вкладку Console (Консоль)

Вставьте туда весь код скрипта и нажмите Enter

Подтвердите выполнение скрипта в появившемся окне

Скрипт начнёт автоматическую прокрутку и удаление всех постов, дожидаясь загрузки новых.

📋 Что делает скрипт
Запрашивает у пользователя подтверждение перед запуском

Ищет и кликает по иконке «троеточие» (...) на каждом посте

Находит в выпадающем меню пункт «Удалить»

Подтверждает удаление

Повторяет действия до тех пор, пока все посты не будут удалены

⏱️ Настройки
В коде есть два параметра задержки:

js
Копировать
Редактировать
const DELAY = 300;        // Задержка между кликами (мс)
const SCROLL_DELAY = 800; // Задержка после прокрутки (мс)
При необходимости можно увеличить их, если интернет медленный или VK тормозит.

✅ Совместимость
✔️ Актуально на июнь 2025 года

✔️ Проверено в Google Chrome

❌ Не работает на чужих страницах (только на вашей стене)
-----------------------------------------------------------------------------------------

(async () => {
  'use strict';

  if (!confirm('Удалить все посты на стене?')) return;

  let deleted = 0;
  const DELAY = 300;       // задержка между кликами (мс)
  const SCROLL_DELAY = 800; // задержка после прокрутки

  function sleep(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
  }

  async function deletePostsBatch() {
    const menuIcons = Array.from(document.querySelectorAll('svg.vkuiIcon--more_horizontal_24'));
    if (menuIcons.length === 0) return false;

    for (const icon of menuIcons) {
      let btn = icon.closest('[role="button"]') || icon.parentElement;
      btn.click();
      await sleep(DELAY);

      const deleteSpan = Array.from(document.querySelectorAll('span.vkitTextClamp__root--23kcq'))
        .find(el => el.textContent.trim() === 'Удалить');

      if (deleteSpan) {
        deleteSpan.closest('div, button').click();
        await sleep(DELAY);

        const confirmBtn = document.querySelector('button[class*="Button"][class*="danger"]');
        if (confirmBtn) {
          confirmBtn.click();
          deleted++;
          console.log(`🗑️ Удалено: ${deleted}`);
          await sleep(DELAY);
        } else {
          console.warn('Кнопка подтверждения удаления не найдена');
        }
      } else {
        console.warn('Пункт "Удалить" не найден в меню');
      }

      // Можно убрать этот клик, если меню закрывается автоматически
      // document.body.click();
      await sleep(DELAY);
    }
    return true;
  }

  while (true) {
    window.scrollTo(0, document.body.scrollHeight);
    await sleep(SCROLL_DELAY);

    const hasPosts = await deletePostsBatch();
    if (!hasPosts) break;
  }

  console.log('✅ Удаление постов завершено. Всего удалено: ' + deleted);
})();

