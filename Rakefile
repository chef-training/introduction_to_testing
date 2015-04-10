def issue_body(number)
"""Scene #{number} - Review / Edit

Review / Edit the text to ensure:

* continuity of voice
* clarity of message
* concise expression

The following sections need to be reviewed:

* [ ] markdown
* [ ] slides
"""
end

task :scene do
  number = ENV['SCENE']
  fail "No scene specified in the environment variable SCENE=" unless number

  1.upto(number.to_i) do |i|
    index = "%02d" % i
    `touch scene_#{index}.md`
  end

  `ghi open --message "#{issue_body(number)}"`
end



