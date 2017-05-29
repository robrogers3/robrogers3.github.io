---
layout: post
title:  "Emacs as a PHP IDE!"
date:   2017-05-28 17:29:02 -0700
categories: emacs php
---
You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

## Requirements for an IDE
* supports php for:
  * correct code style (PSR2)
  * support for namespaces
  * syntax checker
* markup support
  * html,css,blade,twig
* support for javascript
  * code style
  * syntax
* navigating to definitions and symbols
* refactoring tools
* finding files easily
* listing files both in directories and those already opened.
* git integration
* decent syntax highlighting and fonts
* code completion
* snippet support
* exec code inline
* integrated/easy unit testing


## Meeting the requirements.


# The first place to start is a .emacs file

{% highlight lisp %}
;;requires

;syntax and highlighting
(require 'web-mode)
(require 'php-mode)
;syntax checking
(require 'flymake-php)
;refactoring
(require 'php-refactor-mode)
;snippets
(require 'php-auto-yasnippets)
;auto-complete
(require 'company-php)
(require 'ac-php)

;;hooks
(autoload 'php-mode "php-mode" "php editing mode." t)
(add-hook 'php-mode-hook 'flymake-php-load)
(add-hook 'php-mode-hook 'php-refactor-mode)
(add-hook 'php-mode-hook
          '(lambda ()
             (company-mode t)
             (ac-php-core-eldoc-setup) ;; enable eldoc
             (make-local-variable 'company-backends)
             (add-to-list 'company-backends 'company-ac-php-backend)
			 (auto-complete-mode t)
			 (setq ac-sources  '(ac-source-php ) )
			 (yas-global-mode 1)
			 (ac-php-core-eldoc-setup ) ;; enable eldoc

	     ))

(setq web-mode-engines-alist
      '(("php"    . "\\.phtml\\'")
        ("blade"  . "\\.blade\\."))
)

;;mode to file type mapping
(setq auto-mode-alist (cons '("\\.html$" . html-mode) auto-mode-alist))
(setq auto-mode-alist (cons '("\\.php$" . php-mode) auto-mode-alist))
(add-to-list 'auto-mode-alist '("\\.blade\\.php\\'" . web-mode))

{% endhighlight %}

## The packages you need for the functionality

* php-mode
* web-mode
* phpunit
* auto-complete
* epl
* ac-php-core
* ac-php
* company-php
* company
* grizzl
* fiplr grizzl
* flymake-php
* flymake-phpcs
* magit
* php-auto-yasnippets
* yasnippet
* php-cs-fixer
* php-refactor-mode
* xcscope

## The binaries you need to make the packages work
* refactor.phar
* phpcbf
* phpcs
* cscope
* phpunit


Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
