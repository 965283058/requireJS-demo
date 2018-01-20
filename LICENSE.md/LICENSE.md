var REQ_TOTAL = 0;

 window.exports = {};

 var exp_arr = [];


 function isArray(param) {
     return param instanceof Array;
 }


 function require(arr, callback) {

     var req_list;

     if(isArray(arr)) {
         req_list = arr;
     } else {
         req_list = [arr];
     }
     var req_len = req_list.length;


     for(var i=0;i<req_len;i++) {
         var req_item = req_list[i];

         var $script = createScript(req_item, i);

         var $node = document.querySelector('head');

         (function($script) {

             $script.onload = function() {
                 REQ_TOTAL++;

                 var script_index = $script.getAttribute('index');

                 exp_arr[script_index] = exports;

                 window.exports = {};

                 //所有链接加载成功后，执行callback
                 if(REQ_TOTAL == req_len) {
                     callback && callback.apply(exports, exp_arr);


                 }

             }

             $node.appendChild($script);
         })($script);

     }

 }

 function createScript(src, index) {
     var $script = document.createElement('script');

     $script.setAttribute('src', src);
     $script.setAttribute('index', index);

     return $script;
 }
