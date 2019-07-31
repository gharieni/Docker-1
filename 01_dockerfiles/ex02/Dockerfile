FROM ruby
RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y nodejs sqlite3

RUN gem install rails

ONBUILD COPY app /opt/app
ONBUILD WORKDIR /opt/app

ONBUILD RUN bundle install
ONBUILD RUN rake db:migrate
ONBUILD RUN rake db:seed

# docker build -t ft-rails:on-build .
# docker build -t ft-rails:on-build .
# docker run -it --rm -p 3000:3000 ft-rails:on-build
