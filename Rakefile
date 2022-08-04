require 'rubygems'
require 'bundler/setup'
require 'chunky_png'

file '581_720x256_BWR.c' => '581_720x256_BWR.png' do |t|
  puts t.name.inspect

  def colour(pixel)
    r = ChunkyPNG::Color.r(pixel)
    g = ChunkyPNG::Color.g(pixel)
    b = ChunkyPNG::Color.b(pixel)
    a = ChunkyPNG::Color.a(pixel)

    if pixel == 4294967295 || (r == 255 && g == b && g >= 242)
      # white
      return [:white, 255]
    elsif r == g && r == b
      # black / grey
      return [:black, 255 - r]
    elsif r != g && g == 0 && b == 0
      # red
      return [:red, 255]
    elsif r != g && g == b && b < 255
      # red fade
      return [:white, 255]
    else
      puts pixel.inspect
      puts [
        ChunkyPNG::Color.r(pixel),
        ChunkyPNG::Color.g(pixel),
        ChunkyPNG::Color.b(pixel),
        ChunkyPNG::Color.a(pixel)
      ].inspect
      return [:unknown, nil]
    end
  end

  def print_values(image, col)
    image.width.times do |x|
    # 2.times do |x|
      array = []
      image.height.times do |y|
      # 64.times do |y|
        # next unless y % 2 == 0
        y = image.height - 1 - y # Read from the bottom up
        pixel = image[x, y]
        colour_name, value = colour(pixel)

        if colour_name == col
          array << 1
        else
          array << 0
        end
      end
      # puts array.count

      sliced_array = []
      array.each_slice(8).each do |slice|
        # puts slice.inspect
        val = 0
        slice.reverse.each.with_index do |v, i|
          val = val | v << i
        end
        # puts val
        sliced_array << val
      end
      puts sliced_array.count

      puts "#{sliced_array.map{ |n| "0x%02x" % [n] }.join(',')}, // #{x + 1}"
    end
  end

  image = ChunkyPNG::Image.from_file(t.source)

  # puts 'unsigned char const image_581_720x256_BWR_blackBuffer[]='
  # puts '{'
  # print_values(image, :black)
  # puts '};'
  #
  # puts 'unsigned char const image_581_720x256_BWR_redBuffer[]='
  # puts '{'
  # print_values(image, :red)
  # puts '};'
end
