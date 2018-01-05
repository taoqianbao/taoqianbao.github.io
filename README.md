## 项目背景

个人博客

关于HEXO(https://hexo.io/zh-cn/docs/)



## config


``` JS
{
  'title': '致力于拯救世界的IT农民工',
  'subtitle': '朱胜峰的网络事件',
  'description': 'I love programming and I am very passionate about it. I have been coding for 10+ years now. Like C,ASP,PHP,C#,JavaScript...',
  'author': 'Peter Zhu',
  'language': null,
  'timezone': null,
  'url': 'http://taoqianbao.github.io',
  'root': '/',
  'permalink': ':year/:month/:day/:title/',
  'permalink_defaults': null,
  'source_dir': 'source',
  'public_dir': 'public',
  'tag_dir': 'tags',
  'archive_dir': 'archives',
  'category_dir': 'categories',
  'code_dir': 'downloads/code',
  'i18n_dir': ':lang',
  'skip_render': null,
  'new_post_name': ':title.md',
  'default_layout': 'post',
  'titlecase': false,
  'external_link': true,
  'filename_case': 0,
  'render_drafts': false,
  'post_asset_folder': false,
  'relative_link': false,
  'future': true,
  'highlight': {
    'enable': true,
    'auto_detect': false,
    'line_number': true,
    'tab_replace': null
  },
  'default_category': 'uncategorized',
  'category_map': null,
  'tag_map': null,
  'date_format': 'YYYY-MM-DD',
  'time_format': 'HH:mm:ss',
  'per_page': 10,
  'pagination_dir': 'page',
  'theme': 'landscape',
  'deploy': {
    'type': 'git',
    'repo': 'https://github.com/taoqianbao/taoqianbao.github.io.git',
    'branch': 'master'
  },
  'ignore': [
    
  ],
  'index_generator': {
    'per_page': 10,
    'order_by': '-date',
    'path': ''
  },
  'archive_generator': {
    'per_page': 10,
    'yearly': true,
    'monthly': true,
    'daily': false
  },
  'category_generator': {
    'per_page': 10
  },
  'feed': {
    'type': 'atom',
    'limit': 20,
    'hub': '',
    'content': true,
    'content_limit': 140,
    'content_limit_delim': '',
    'path': 'atom.xml'
  },
  'tag_generator': {
    'per_page': 10
  },
  'marked': {
    'gfm': true,
    'pedantic': false,
    'sanitize': false,
    'tables': true,
    'breaks': true,
    'smartLists': true,
    'smartypants': true,
    'modifyAnchors': '',
    'autolink': true
  },
  'server': {
    'port': 4000,
    'log': false,
    'ip': '0.0.0.0',
    'compress': false,
    'header': true
  }
}

```