---
layout: post
title:  "first post"
date:   2020-08-09 11:09:00 +1000
categories: code bits
---

I have an aspnetcore 3.1 and angular 8 app, which has some C# models, such as

{% highlight code %}
    public class Item
    {
        public int Id { get; set; }
        public int Index { get; set; }
        public int ItemType { get; set; }
        public string Label { get; set; }
        public string Data { get; set; }
        public string ExpandedIcon { get; set; }
        public string CollapsedIcon { get; set; }
        public ICollection<Item> Children { get; set; }
        public string Response { get; set; }
        public Exam Exam { get; set; }
    }
{% endhighlight %}

These have fields included from PrimeNG's TreeNode object.
 
Controllers with get methods like

{% highlight code %}
[HttpGet("{examId}")]
        public IActionResult GetItemsForExam(int examId)
        {
            try
            {
                return Ok(_mapper.Map<IEnumerable<Item>, IEnumerable<ItemVM>>(_repo.GetItemsForExam(examId)));
            }
            catch (Exception ex)
            {
                _logger.LogError($"Failed to get items for exam: {ex}");
                return BadRequest("Failed to get items for exam");
            }
        }
{% endhighlight %}

Post method like so

{% highlight code %}
[HttpPost]
        public void Post([FromBody] ItemVM model)
        {
            try
            {
                if (ModelState.IsValid)
                {
                    var rootItem = _mapper.Map<ItemVM, Item>(model);
                   ICollection<Item> children = rootItem.Children;
                  foreach (var item in children)
                      {
                        _repo.AddItem(item, rootItem.Exam);
                        _repo.SaveAll();
                      } 
                }
                
            }
            catch (Exception ex)
            {
                _logger.LogError($"Failed to save a new item: {ex}");
            }
        }
{% endhighlight %}

Repository pattern -- with an AddItem method referenced (from the Post method in the Controller) like so:

{% highlight code %}
public void AddItem(Item item, Exam examRef)
        {
           Exam exam = _ctx.Exams
                .Where(e => e.Id == examRef.Id)
                .FirstOrDefault();
            item.Exam = exam;
            exam.Items = (ICollection<Item>)GetItemsForExam(exam.Id);
            exam.Items.Add(item);
            _ctx.Add(item);

        }
		
{% endhighlight %}

TBC