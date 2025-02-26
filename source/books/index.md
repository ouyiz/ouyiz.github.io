---
title: 书籍
date: 2023-10-01

---

<style>
  #booklist {
    padding: 20px;
  }

  .book-card {
    display: flex;
    flex-direction: column;
    background: rgba(255, 255, 255, 0.1);
    border: 1px solid #a5a5a5ee;
    border-radius: 10px;
    padding: 15px;
    margin: 10px;
    width: calc(48% - 7px);
    transition: all 0.25s ease;
    user-select: none;
  }

  @media screen and (max-width: 800px) {
    .book-card {
      width: 100%;
    }
  }

  .book-card:hover {
    box-shadow: 0 5px 10px 8px #07111b29;
    transform: translateY(-3px);
  }

  .book-card .cover {
    width: 100%;
    height: 200px;
    border-radius: 8px;
    overflow: hidden;
  }

  .book-card .cover img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  .book-card .info {
    padding-top: 15px;
  }

  .book-card .info h3 {
    font-size: 18px;
    font-weight: 700;
    margin-bottom: 10px;
  }

  .book-card .info p {
    font-size: 14px;
    color: #676767;
    margin-bottom: 10px;
  }

  .book-card .info .author {
    font-size: 13px;
    color: #333;
    font-weight: 500;
  }

  .book-card .info .tag {
    margin-top: 10px;
    padding: 4px 8px;
    font-size: 12px;
    background-color: #f0f0f0;
    border-radius: 5px;
    color: #666;
  }

  #booklist .bb-info {
    font-weight: 700;
    font-size: 22px;
  }

  #booklist .card-header {
    display: flex;
    align-items: center;
  }

  #booklist .card-header .avatar {
    width: 32px;
    height: 32px;
    border-radius: 50%;
    margin-right: 10px;
    overflow: hidden;
  }

  #booklist .card-header svg {
    height: 20px;
    width: 20px;
    margin-left: 5px;
  }

  .loading img {
    border-radius: 15px;
  }
</style>

<div id="booklist">
  <div class="bb-info">我的书单</div>
  <div id="bb-main">
    // 循环显示书籍卡片
    each book, index in books
      .book-card
        .cover
          img(src=book.cover, alt=book.title)
        .info
          h3= book.title
          p= book.description
          .author 作者: #{book.author}
          .tag #{book.tag}
  </div>
</div>

<script src="/js/booklist.js"></script>
