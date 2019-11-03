---
layout: single
featured: true
title: "Array#bsearch"
categories:
  - todayilearned
---

    bsearch {|x| block } → elem

Tham khảo: [devdocs.io](https://devdocs.io/ruby~2.5/array#method-i-bsearch)

You can use this method in **two modes**: a find-minimum mode and a find-any mode. In either case, the elements of the array must be monotone (or **sorted**) with respect to the block.
> Lưu ý 2 chỗ in đậm. 2 modes phụ thuộc vào block truyền vào. sorted theo thứ tự asc hay desc cũng ảnh hưởng đến kết quả. Sau đây sẽ nói rõ hơn.

### Mode đầu tiên là **find-minimum mode**, khi block trả về giá trị true/false.

Ví dụ 1:

    ary = [0, 4, 7, 10, 12]
    ary.bsearch {|x| x >= 4 } #=> 4

`x >= 4` là một block trả về true/false.

- **middle element** là 7. (middle = (n/2).floor)
- `7 >= 4` returns true.
- xét mảng bên **trái** tính từ middle element, tức là `[0, 4]` . Nếu block trả về là false thì xét mảng bên phải
- middle element của mảng mới là 0
- `0 >= 4` returns false.
- xét mảng bên **phải** tính từ 0, tức là `[4]`.
- middle element của mảng mới là 4
- ` 0 >= 4` returns true.
- xét mảng bên **trái** tính từ middle element, lúc này không còn phần tử nào nữa. Return middle element gần nhất trả về true, tức là 4, nếu không có middle element nào thì return nil.

Ví dụ 2:

    statuses  =  ["Cancelled",  "Completed",  "Open",  "Options",  "Shipped"]  
    statuses.bsearch  {  |status|  status  ==  "Completed"  }
    => nil
   
Ủa @@

- `==` là operator trả về true/false. OK, vậy đây là mode 1
- Middle element: "Open".
- `"Open" == "Completed" # => false`. 
- xét mảng bên phải `["Options", "Shipped"]`
- thôi nhìn là biết dù có thực hiện phân tích nữa thì kết quả cũng chỉ trả về nil thôi.

À thì có thể là vì mảng được sắp xếp theo asc, nếu sắp xếp ngược lại thì sao? (**)

    statuses = ["Shipped", "Options", "Open", "Completed", "Cancelled"]
  
  - Middle: 'Open'.
  - false -> `["Completed", "Cancelled"]` (*)
  - Middle: 'Cancelled', tại sao: vì (n/2).floor = 1 -> phần tử thứ 2 
  - false -> `[Cancelled]`
  - vậy kết quả vẫn sẽ là nil thôi.

Ủa vậy làm sao để return "Completed" đây. 

- Nếu để ý thì với ví dụ ngay bên trên, nếu bên phải của "Open", chỉ có "Completed", thì mảng được xét ở (*) chỉ có một phần tử là "Completed" thôi. Nên lúc đó giá trị trả về sẽ là "Completed". (Return middle element gần nhất trả về true).
- Xử dụng mode thứ 2 của binary search. Xin mời đọc tiếp.

### Mode thứ hai là **find-any mode**, khi block trả về giá trị số. 

Lấy luôn ví dụ trên:

    statuses  =  ["Cancelled",  "Completed",  "Open",  "Options",  "Shipped"]  
    statuses.bsearch  {  |status|  status  <=>  "Completed"  }
    => nil

Ơ đm vẫn nil à @@. Bình tĩnh đã ô eii =))

-	Operator `<=>` trả về các giá trị số (-1, 0, 1). Chính xác là block này trả về kiểu số, nên đây là mode thứ 2.
-	Middle vẫn là "Open".
-	`"Open" <=> "Completed"` returns 1. Tìm bên phải: `["Options", "Shipped"]`. Nếu là -1 thì tìm bên trái, mà 0 thì return luôn.

À thì ra là thế, thế tôi biết rồi, giờ thay đổi thứ tự của statuses chiều ngược lại là tìm được Completed chứ gì. Ez!

Thử nhé	

    statuses = ["Shipped", "Options", "Open", "Completed", "Cancelled"]
    => ["Shipped", "Options", "Open", "Completed", "Cancelled"]
    statuses.bsearch  {  |status|  status  <=>  "Completed"  }
    => "Completed"

Thế thì bình thường. Đỉnh hơn đây này: chỉ cần thay đổi block 1 chút.

    statuses  =  ["Cancelled",  "Completed",  "Open",  "Options",  "Shipped"]  
    statuses.bsearch  {  |status|  "Completed"  <=>  status  }
    => "Completed"

Vì mode thứ 2 này có những 3 giá trị trả về  (-1, 0, 1), cho nên sẽ không bị miss case mặc dù đã tìm đc path đúng như ở ví dụ (**)

Cần thêm ví dụ không?

    ary = [0, 4, 7, 10, 12]
    # try to find v such that 4 <= v < 8
    ary.bsearch {|x| 1 - x / 4 } #=> 4 or 7
    # try to find v such that 8 <= v < 10
    ary.bsearch {|x| 4 - x / 2 } #=> nil

Hoá ra bsearch trong ruby cũng dễ. Cần quan tâm đến mode muốn sử dụng và thứ tự sắp xếp của mảng đã cho thôi.

Khi cần return index của phần tử muốn tìm kiếm, thay vì bản thân phần tử đó thì dùng hàm `bsearch_index` nhé!

