---
layout: default
title: Vijayshinva Karnure - Blog
permalink: /blog/
---


<div class="container-fluid dotted-background">
	<div class="container container-left">
	    <div class="row">
	        <div class="col-md-3 hidden-xs">
				<div class="sidebar well" style="background-color: #FFF">
				    <h3 class="title with-icon">
						<span class="fa fa-calendar cat-title"></span>
						Archive
					</h3>
					{% for post in site.posts  %}
					    {% capture this_year %}{{ post.date | date: "%Y" }}{% endcapture %}
					    {% capture this_month %}{{ post.date | date: "%B" }}{% endcapture %}
					    {% capture next_year %}{{ post.previous.date | date: "%Y" }}{% endcapture %}
					    {% capture next_month %}{{ post.previous.date | date: "%B" }}{% endcapture %}

					    {% if forloop.first %}
					    <ul class="nav nav-list blogarchive">
            				<li><label class="tree-toggler nav-header">{{ this_month }} {{this_year}}</label>
            					<ul class="nav nav-list tree">
					    {% endif %}

					    <li><a href="{{ post.url }}">{{ post.title }}</a></li>

					    {% if forloop.last %}
					    		</ul>
					    	</li>
					    </ul>
					    {% else %}
					        {% if this_year != next_year %}
					        	</ul>
					        </li>
					        <li><label class="tree-toggler nav-header">{{ next_month }} {{next_year}}</label>
            					<ul class="nav nav-list tree">
					        {% else %}    
					            {% if this_month != next_month %}
					            </ul>
					        <li><label class="tree-toggler nav-header">{{ next_month }} {{next_year}}</label>
					            <ul class="nav nav-list tree">
					            {% endif %}
					        {% endif %}
					    {% endif %}
					{% endfor %}
				    <!--{% for post in site.posts  %}
					    {% capture this_year %}{{ post.date | date: "%Y" }}{% endcapture %}
					    {% capture this_month %}{{ post.date | date: "%B" }}{% endcapture %}
					    {% capture next_year %}{{ post.previous.date | date: "%Y" }}{% endcapture %}
					    {% capture next_month %}{{ post.previous.date | date: "%B" }}{% endcapture %}

					    {% if forloop.first %}
					    <h2 id="{{ this_year }}-ref">{{this_year}}</h2>
					    <h3 id="{{ this_year }}-{{ this_month }}-ref">{{ this_month }}</h3>
					    <ul>
					    {% endif %}

					    <li><a href="{{ post.url }}">{{ post.title }}</a></li>

					    {% if forloop.last %}
					    </ul>
					    {% else %}
					        {% if this_year != next_year %}
					        </ul>
					        <h2 id="{{ next_year }}-ref">{{next_year}}</h2>
					        <h3 id="{{ next_year }}-{{ next_month }}-ref">{{ next_month }}</h3>
					        <ul>
					        {% else %}    
					            {% if this_month != next_month %}
					            </ul>
					            <h3 id="{{ this_year }}-{{ next_month }}-ref">{{ next_month }}</h3>
					            <ul>
					            {% endif %}
					        {% endif %}
					    {% endif %}
					{% endfor %}-->
				</div>
				
			</div>
			<div class="col-md-9">
				<div>
					{% for post in site.posts %}
					<div class="panel panel-default">
						<div class="panel-body">
					    	<h3 class="media-heading"><a class="text-dblue" href="{{ post.url }}">{{ post.title }}</a></h3>
					    	<i class="fa fa-clock-o text-muted"></i> <time class="media-meta text-muted" datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%b %-d, %Y" }}</time>
					    	{{ post.excerpt }} 
					    
				    		{% capture content_words %} 
						      {{ post.content | number_of_words }} 
						    {% endcapture %} 
						    {% capture excerpt_words %} 
					   			{{ post.excerpt | number_of_words }} 
						    {% endcapture %} 
							{% if excerpt_words != content_words %}
							    <p><a class="btn btn-sm btn-primary" href="{{ post.url }}#read-more" role="button">Read more <i class="fa fa-arrow-circle-right"></i></a>
							    </p>
					    	{% endif %}
					  	</div>
					  	<div class="panel-footer">
						    <i class="fa fa-tags text-primary"></i>
						    {% assign taglist = post.tags | sort %}
						    {% for tag in taglist | order: descending %}
						    <small>
							    <!--<a class="text-dblue" href="/tags/#{{ tag | cgi_escape }}" title="View posts tagged with &quot;{{ tag }}&quot;">{{ tag }}</a>-->
							    {{ tag }}
					      		{% if forloop.last != true %}, {% endif %}
					    	</small> 
					   		{% endfor %}
					  	</div>
					</div>
				    {% endfor %}
				</div>
			</div>
		</div>
	</div>
</div>
<script type="text/javascript">
	$(document).ready(function () {
	$('label.tree-toggler').click(function () {
		$(this).parent().children('ul.tree').toggle(150);
	});
});
</script>